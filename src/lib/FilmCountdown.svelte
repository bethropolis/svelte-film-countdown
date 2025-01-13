<script>
	let {
		initialCount = $bindable(5),
		countdownDuration = $bindable(1000), // Note: in milliseconds
		onComplete = () => {}, // Callback when countdown finishes
		onEachCount = () => {}, // Callback after each count
		config = $bindable({
			numberColor: 'var(--countdown-number-color)',
			numCircles: 2,
			circleRadius: 64,
			circleSpacing: 8, // Default spacing between circles
			strokeRatio: 1 / 40, // Proportion of radius for all strokes
			canvasSize: 256 // New config for viewBox dimensions
		}),
		loop = $bindable(false)
	} = $props();

	let count = $state(initialCount);
	let progress = $state(0);
	let isRunning = $state(false);
	let isPaused = $state(false);
	let pauseStartTime = $state(0); // To store the timestamp when paused
	let pausedDuration = $state(0); // To store the total duration of pauses

	/**
	 * Starts the countdown
	 * @returns {void}
	 */
	export function start() {
		reset();
		isRunning = true;
	}

	/**
	 * Pauses the countdown
	 * @returns {void}
	 */
	export function pause() {
		if (isRunning && !isPaused) {
			isPaused = true;
			pauseStartTime = performance.now();
		}
	}

	/**
	 * Resumes the countdown
	 * @returns {void}
	 */
	export function resume() {
		if (isPaused) {
			isPaused = false;
			pausedDuration += performance.now() - pauseStartTime; // Accumulate paused time
		}
	}

	/**
	 * Resets the countdown
	 * @returns {void}
	 */
	export function reset() {
		isPaused = false;
		count = initialCount;
		progress = 0;
		isRunning = false;
		pausedDuration = 0;
	}

	$effect(() => {
		let startTime;
		let animationFrame;

		const animate = (timestamp) => {
			if (!isRunning) return;

			if (isPaused) {
				return; // Do nothing if paused
			}

			if (!startTime) {
				startTime = timestamp - pausedDuration; // Adjust start time by total paused duration
			}

			const elapsed = timestamp - startTime;
			const newProgress = (elapsed % countdownDuration) / countdownDuration;

			progress = newProgress;

			if (elapsed >= countdownDuration) {
				startTime = timestamp;
				if (count > 1) {
					count--;
					onEachCount(count);
				} else {
					count = 0;
					isRunning = false;
					onComplete();
					if (loop) {
						start();
					}
				}
			}

			if (isRunning) {
				animationFrame = requestAnimationFrame(animate);
			}
		};

		if (isRunning) {
			animationFrame = requestAnimationFrame(animate);
		}

		return () => {
			if (animationFrame) {
				cancelAnimationFrame(animationFrame);
			}
		};
	});

	const circles = $derived(Array(config.numCircles ?? 2).fill(null));
	const containerHeight = $derived(((config.canvasSize ?? 256) * 9) / 16);
	const canvasSize = $derived(config.canvasSize ?? 256);
	const circleSpacing = $derived(config.circleSpacing ?? 8);
	const circleRadius = $derived(config.circleRadius ?? 60);
	const strokeRatio = $derived(config.strokeRatio ?? 1 / 40);


	/**
	 * Calculates the SVG path data for a sweeping background arc.
	 *
	 * @param {Object} params - The parameters for the calculation.
	 * @param {number} params.angle - The angle of the arc in radians.
	 * @param {number} params.cx - The x-coordinate of the center of the arc.
	 * @param {number} params.cy - The y-coordinate of the center of the arc.
	 * @param {number} params.r - The radius of the arc.
	 * @returns {string} The SVG path data for the sweeping background arc.
	 */
	function calculateSweepingBackgroundPoints({ angle, cx, cy, r }) {
		const endX = cx + r * Math.cos(angle - Math.PI / 2);
		const endY = cy + r * Math.sin(angle - Math.PI / 2);
		const largeArcFlag = angle > Math.PI ? 1 : 0;

		return `M ${cx} ${cy} L ${cx} ${cy - r} A ${r} ${r} 0 ${largeArcFlag} 1 ${endX} ${endY} Z`;
	}

	/**
	 * A derived store that calculates the SVG path data for the sweeping background arc
	 * based on the current canvas size, container height, and progress.
	 * 
	 * @returns {string} The SVG path data for the sweeping background arc.
	 */
	const sweepingBackgroundPath = $derived(() => {
		const w = canvasSize;
		const h = containerHeight;
		const cx = w / 2;
		const cy = h / 2;
		const angle = progress * 2 * Math.PI;
		const r = (Math.max(w, h) / 2) * Math.sqrt(2); // Radius to cover the corners
		return calculateSweepingBackgroundPoints({ angle, cx, cy, r });
	});
</script>

<div class="film-countdown">
	<div class="film-holes">
		<svg width="100%" height="100%">
			<pattern
				id="filmHolesPattern"
				x="0"
				y="0"
				width="var(--film-hole-width)"
				height="100%"
				patternUnits="userSpaceOnUse"
			>
				<rect
					x="0"
					y="var(--film-hole-y-top)"
					width="var(--film-hole-rect-width)"
					height="var(--film-hole-rect-height)"
					fill="var(--film-hole-color)"
				/>
				<rect
					x="0"
					y="calc(100% - var(--film-hole-y-bottom))"
					width="var(--film-hole-rect-width)"
					height="var(--film-hole-rect-height)"
					fill="var(--film-hole-color)"
				/>
			</pattern>
			<rect width="100%" height="100%" fill="url(#filmHolesPattern)" />
		</svg>
	</div>

	<div class="countdown-container">
		<svg viewBox={`0 0 ${canvasSize} ${containerHeight}`}>
			<filter id="noiseFilter">
				<feTurbulence
					type="fractalNoise"
					baseFrequency="0.8"
					numOctaves="3"
					seed={Math.random() * 100}
				/>
				<feColorMatrix type="saturate" values="0" />
				<feBlend in="SourceGraphic" mode="multiply" />
			</filter>

			<path
				class="sweeping-background"
				d={sweepingBackgroundPath()}
				fill="var(--sweeping-background-color)"
			/>

			{#each circles as _, i}
				<circle
					cx={canvasSize / 2}
					cy={containerHeight / 2}
					r={circleRadius - i * circleSpacing}
					fill="none"
					stroke="url(#circleGradient)"
					stroke-width={circleRadius * strokeRatio}
				/>
			{/each}

			<defs>
				<linearGradient id="circleGradient" x1="0%" y1="0%" x2="100%" y2="100%">
					<stop
						offset="0%"
						stop-color={config.circleGradientStart ?? 'var(--circle-gradient-start)'}
						stop-opacity="1"
					/>
					<stop
						offset="50%"
						stop-color={config.circleGradientMiddle ?? 'var(--circle-gradient-middle)'}
						stop-opacity="var(--circle-gradient-middle-opacity)"
					/>
					<stop
						offset="100%"
						stop-color={config.circleGradientEnd ?? 'var(--circle-gradient-end)'}
						stop-opacity="1"
					/>
				</linearGradient>
			</defs>
		</svg>

		<div class="number-display">
			<span
				class="number-text"
				style="color: {config.numberColor}; font-family: var(--number-font); text-shadow: var(--number-text-shadow);"
			>
				{count === 0 ? '' : count}
			</span>
		</div>
	</div>

	<div class="grid-lines">
		<div
			class="vertical-line"
			style="background-color: var(--grid-line-color); box-shadow: 0 0 1px var(--grid-line-color);"
		></div>
		<div
			class="horizontal-line"
			style="background-color: var(--grid-line-color); box-shadow: 0 0 1px var(--grid-line-color);"
		></div>
	</div>

	<div
		class="film-grain"
		style="opacity: var(--film-grain-opacity); background-image: var(--film-grain-pattern);"
	></div>
	<div
		class={`flicker-overlay ${isRunning ? 'animate-flicker' : ''}`}
		style="animation-duration: var(--flicker-duration);"
	></div>

	<div class="age-artifacts">
		<div
			class="top-vignette"
			style="background-image: linear-gradient(to bottom, rgba(0, 0, 0, var(--vignette-strength)), transparent);"
		></div>
		<div
			class="bottom-vignette"
			style="background-image: linear-gradient(to top, rgba(0, 0, 0, var(--vignette-strength)), transparent);"
		></div>
		<div
			class="center-vignette"
			style="background-image: radial-gradient(circle at 50% 50%, rgba(0, 0, 0, var(--center-vignette-strength)), transparent);"
		></div>
	</div>

	<div class="scratches" style="opacity: var(--scratch-opacity);">
		{#each Array.from({ length: 3 }) as _, i}
			<div
				class="scratch"
				style="left: {Math.random() * 100}%; top: {Math.random() * 100}%; height: {10 +
					Math.random() * 20}px; transform: rotate({Math.random() *
					360}deg); background-color: var(--scratch-color);"
			></div>
		{/each}
	</div>
</div>

<style>
	:root {
		--countdown-number-color: black;
		--countdown-button-color: rgba(0, 0, 0, 0.8);
		--countdown-button-text-color: white;

		--film-hole-width: 20px;
		--film-hole-rect-width: 8px;
		--film-hole-rect-height: 12px;
		--film-hole-y-top: 10px;
		--film-hole-y-bottom: 22px;
		--film-hole-color: black;

		--sweeping-background-color: rgba(0, 0, 0, 0.15);

		--circle-gradient-start: black;
		--circle-gradient-middle: black;
		--circle-gradient-middle-opacity: 0.8;
		--circle-gradient-end: black;

		--grid-line-color: rgba(255, 255, 255, 0.3); /* More subtle grid lines */
		--film-grain-opacity: 0.1; /* Slightly reduced grain */
		--flicker-duration: 0.15s;
		--vignette-strength: 0.4; /* Darker vignette */
		--center-vignette-strength: 0.15;
		--scratch-color: rgba(255, 255, 255, 0.2);
		--scratch-opacity: 0.15;
		--number-font: monospace;
		--number-text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
		--film-grain-pattern: linear-gradient(
			to bottom,
			rgba(156, 163, 175, 0.05),
			transparent,
			rgba(156, 163, 175, 0.05)
		); /* More subtle grain pattern */
	}

	.film-countdown {
		position: relative;
		width: 100%;
		max-width: 42rem;
		min-width: 320px;
		aspect-ratio: 16 / 9;
		background-color: #d1d5db;
		overflow: hidden;
	}

	.film-holes {
		position: absolute;
		inset: 0;
		opacity: 0.1;
	}

	.countdown-container {
		position: absolute;
		inset: 0;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.number-display {
		position: absolute;
		inset: 0;
		display: flex;
		align-items: center;
		justify-content: center;
		user-select: none; /* Make the number unselectable */
	}

	.number-text {
		color: var(--countdown-number-color);
		font-size: 8rem;
		font-family: var(--number-font);
		text-shadow: var(--number-text-shadow);
		filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.05));
	}

	.grid-lines {
		position: absolute;
		inset: 0;
		pointer-events: none;
	}

	.vertical-line {
		position: absolute;
		top: 0;
		bottom: 0;
		left: 50%;
		width: 1px;
		background-color: var(--grid-line-color);
		box-shadow: 0 0 1px var(--grid-line-color);
		transform: translateX(-50%);
	}

	.horizontal-line {
		position: absolute;
		left: 0;
		right: 0;
		top: 50%;
		height: 1px;
		background-color: var(--grid-line-color);
		box-shadow: 0 0 1px var(--grid-line-color);
		transform: translateY(-50%);
	}

	.film-grain {
		position: absolute;
		inset: 0;
		background-image: var(--film-grain-pattern);
		mix-blend-mode: overlay;
		pointer-events: none;
		opacity: var(--film-grain-opacity);
	}

	.flicker-overlay {
		position: absolute;
		inset: 0;
		background-color: rgba(0, 0, 0, 0.05);
		pointer-events: none;
		animation-duration: var(--flicker-duration);
	}

	.animate-flicker {
		animation: flicker 0.15s infinite;
	}

	@keyframes flicker {
		0% {
			opacity: 0.92;
		}
		25% {
			opacity: 0.95;
		}
		50% {
			opacity: 0.9;
		}
		75% {
			opacity: 0.93;
		}
		100% {
			opacity: 0.92;
		}
	}

	.age-artifacts {
		position: absolute;
		inset: 0;
		pointer-events: none;
	}

	.top-vignette {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 3px;
		background-image: linear-gradient(
			to bottom,
			rgba(0, 0, 0, var(--vignette-strength)),
			transparent
		);
	}

	.bottom-vignette {
		position: absolute;
		bottom: 0;
		left: 0;
		width: 100%;
		height: 3px;
		background-image: linear-gradient(to top, rgba(0, 0, 0, var(--vignette-strength)), transparent);
	}

	.center-vignette {
		position: absolute;
		inset: 0;
		background-image: radial-gradient(
			circle at 50% 50%,
			rgba(0, 0, 0, var(--center-vignette-strength)),
			transparent
		);
	}

	.scratches {
		position: absolute;
		inset: 0;
		pointer-events: none;
		opacity: var(--scratch-opacity);
	}

	.scratch {
		position: absolute;
		top: 0;
		left: 0;
		width: 1px;
		background-color: var(--scratch-color);
	}
</style>
