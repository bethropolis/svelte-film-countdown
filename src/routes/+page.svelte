<script>
	import FilmCountdown from '$lib/FilmCountdown.svelte';
	import Blooper from '../asserts/Blooper.mp3';

	let audio = new Audio(Blooper);

	// --- Reactive State ---
	let initialCount = $state(5);
	let countdownDuration = $state(1000);
	let loop = $state(false);
	let config = $state({
		numCircles: 2,
		circleRadius: 60,
		circleSpacing: 8,
		strokeRatio: 0.02,
		canvasSize: 256,
		paperTextureOpacity: 0.25,
		paperTextureEnabled: true,
		paperTextureColor: '#a08060',
		paperTextureBlendMode: 'multiply',
		paperTextureIntensity: 0.7
	});
	let beep = $state(false);
	let showAdvancedOptions = $state(false);

	let countdownInstance;

	// --- Event Handlers ---
	async function handleComplete() {
		if (beep) {
			await audio.play();
		}
		console.log('Countdown finished!');
	}

	async function handleEachCount(currentCount) {
		if (beep) {
			await audio.play();
		}
		console.log('Current count:', currentCount);
	}

	// --- Control Functions ---
	function startCountdown() {
		countdownInstance?.start();
	}

	function pauseCountdown() {
		countdownInstance?.pause();
	}

	function resumeCountdown() {
		countdownInstance?.resume();
	}

	function resetCountdown() {
		countdownInstance?.reset();
	}
</script>

<!-- Main Content -->
<main class="p-8 w-full flex flex-col items-center flex-grow justify-center relative">
	<!-- Film Countdown Component -->
	<div
		class="relative w-[320px] md:w-[400px] lg:w-[512px] transform scale-125 md:scale-150 lg:scale-175 transition-transform duration-300"
	>
		<FilmCountdown
			bind:this={countdownInstance}
			{initialCount}
			{countdownDuration}
			{loop}
			onComplete={handleComplete}
			onEachCount={handleEachCount}
			bind:config
		/>
	</div>
</main>

<!-- Footer - Controls -->
<footer class="bg-cream p-4 w-full border-t border-sepia/30 shadow-inner">
	<p class="text-sepia text-center mb-6 vintage-instruction">
		Adjust the settings to see the countdown change.
	</p>

	<!-- Basic Settings in Single Line -->
	<div class="flex items-center justify-center gap-6 mb-4 flex-wrap">
		<div class="flex items-center">
			<label
				for="initialCount"
				class="text-sm font-medium text-sepia mr-2"
				title="Starting number"
			>
				Count:
			</label>
			<input
				type="number"
				id="initialCount"
				bind:value={initialCount}
				class="vintage-input"
			/>
		</div>

		<div class="flex items-center">
			<label
				for="countdownDuration"
				class="text-sm font-medium text-sepia mr-2"
				title="Time in milliseconds for each count"
			>
				Duration (ms):
			</label>
			<input
				type="number"
				id="countdownDuration"
				bind:value={countdownDuration}
				class="vintage-input"
			/>
		</div>

		<!-- Buttons with Vintage Colors -->
		<div class="flex gap-4 flex-wrap justify-center">
			<button
				onclick={startCountdown}
				class="vintage-button vintage-button-primary"
			>
				Start
			</button>
			<button
				onclick={pauseCountdown}
				class="vintage-button vintage-button-secondary"
			>
				Pause
			</button>
			<button
				onclick={resumeCountdown}
				class="vintage-button vintage-button-tertiary"
			>
				Resume
			</button>
			<button
				onclick={resetCountdown}
				class="vintage-button vintage-button-danger"
			>
				Reset
			</button>
		</div>
	</div>

	<!-- Advanced Options Toggle -->
	<div class="text-center mb-4">
		<button
			type="button"
			class="vintage-toggle-button"
			onclick={() => (showAdvancedOptions = !showAdvancedOptions)}
		>
			{showAdvancedOptions ? 'Hide Advanced Options' : 'Show Advanced Options'}
		</button>
	</div>

	{#if showAdvancedOptions}
		<div class="border-t border-sepia/30 pt-4">
			<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 justify-items-start">
				<div class="flex items-center">
					<label
						for="numberColor"
						class="text-sm font-medium text-sepia mr-2"
						title="Color of the countdown number">Color:</label
					>
					<input type="color" id="numberColor" bind:value={config.numberColor} class="h-8 vintage-color-input" />
				</div>
				<div class="flex items-center">
					<label
						for="numCircles"
						class="text-sm font-medium text-sepia mr-2"
						title="Number of concentric circles">Circles:</label
					>
					<input
						type="number"
						id="numCircles"
						bind:value={config.numCircles}
						class="vintage-input"
					/>
				</div>
				<div class="flex items-center">
					<label
						for="circleRadius"
						class="text-sm font-medium text-sepia mr-2"
						title="Radius of the largest circle">Radius:</label
					>
					<input
						type="number"
						id="circleRadius"
						bind:value={config.circleRadius}
						class="vintage-input"
					/>
				</div>
				<div class="flex items-center">
					<label
						for="strokeRatio"
						class="text-sm font-medium text-sepia mr-2"
						title="Thickness of the circle lines">Stroke:</label
					>
					<input
						type="number"
						id="strokeRatio"
						bind:value={config.strokeRatio}
						step="0.01"
						class="vintage-input"
					/>
				</div>
				<div class="flex items-center">
					<label
						for="circleSpacing"
						class="text-sm font-medium text-sepia mr-2"
						title="Space between the circles">Spacing:</label
					>
					<input
						type="number"
						id="circleSpacing"
						bind:value={config.circleSpacing}
						class="vintage-input"
					/>
				</div>
				<div class="flex items-center">
					<label
						for="canvasSize"
						class="text-sm font-medium text-sepia mr-2"
						title="Overall size of the countdown SVG">Canvas:</label
					>
					<input
						type="number"
						id="canvasSize"
						bind:value={config.canvasSize}
						class="vintage-input"
					/>
				</div>
				<div class="flex items-center">
					<input
						type="checkbox"
						id="loop"
						bind:checked={loop}
						class="vintage-checkbox"
					/>
					<label for="loop" class="ml-2 text-sm font-medium text-sepia">Loop</label>
				</div>
				<div class="flex items-center">
					<input
						type="checkbox"
						id="beep"
						bind:checked={beep}
						class="vintage-checkbox"
					/>
					<label for="beep" class="ml-2 text-sm font-medium text-sepia">Beep</label>
				</div>
			

				<!-- Paper Texture Section -->
				<div class="col-span-1 sm:col-span-2 md:col-span-3 lg:col-span-4 mb-2 border-b border-sepia/20 pb-2">
					<h3 class="text-sm font-medium text-sepia mb-2">Paper Texture Settings</h3>
				</div>
				
				<div class="flex items-center">
					<input
						type="checkbox"
						id="paperTextureEnabled"
						bind:checked={config.paperTextureEnabled}
						class="vintage-checkbox"
					/>
					<label for="paperTextureEnabled" class="ml-2 text-sm font-medium text-sepia">Enable Texture</label>
				</div>
				
				<div class="flex items-center">
					<label
						for="paperTextureOpacity"
						class="text-sm font-medium text-sepia mr-2"
						title="Opacity of paper texture effect">Opacity:</label
					>
					<input
						type="number"
						id="paperTextureOpacity"
						bind:value={config.paperTextureOpacity}
						min="0"
						max="1"
						step="0.05"
						class="vintage-input"
					/>
				</div>
				
				<div class="flex items-center">
					<label
						for="paperTextureColor"
						class="text-sm font-medium text-sepia mr-2"
						title="Color of the paper texture">Color:</label
					>
					<input 
						type="color" 
						id="paperTextureColor" 
						bind:value={config.paperTextureColor} 
						class="h-8 vintage-color-input" 
					/>
				</div>
				
				<div class="flex items-center">
					<label
						for="paperTextureIntensity"
						class="text-sm font-medium text-sepia mr-2"
						title="Intensity of the texture effect">Intensity:</label
					>
					<input
						type="number"
						id="paperTextureIntensity"
						bind:value={config.paperTextureIntensity}
						min="0"
						max="1"
						step="0.1"
						class="vintage-input"
					/>
				</div>
				
				<div class="flex items-center">
					<label
						for="paperTextureBlendMode"
						class="text-sm font-medium text-sepia mr-2"
						title="Blend mode for texture">Blend:</label
					>
					<select 
						id="paperTextureBlendMode" 
						bind:value={config.paperTextureBlendMode} 
						class="vintage-input" 
						style="width: auto;"
					>
						<option value="multiply">Multiply</option>
						<option value="overlay">Overlay</option>
						<option value="soft-light">Soft Light</option>
						<option value="hard-light">Hard Light</option>
						<option value="color-burn">Color Burn</option>
					</select>
				</div>
			</div>
		</div>
	{/if}
</footer>

<style>
	/* Vintage UI Elements */
	.vintage-input {
		background-color: rgba(248, 240, 227, 0.8);
		border: 1px solid var(--sepia);
		border-radius: 3px;
		padding: 4px 8px;
		width: 80px;
		font-size: 0.9rem;
		color: var(--sepia);
		box-shadow: inset 1px 1px 3px rgba(0, 0, 0, 0.1);
		transition: all 0.3s ease;
	}

	.vintage-input:focus {
		outline: none;
		border-color: var(--aged-paper);
		box-shadow: 0 0 0 2px rgba(112, 66, 20, 0.2);
	}

	.vintage-button {
		font-weight: bold;
		padding: 6px 12px;
		border-radius: 3px;
		color: var(--sepia);
		border: 1px solid var(--sepia);
		background: var(--aged-paper);
		box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
		transition: all 0.3s ease;
		min-width: 80px;
		letter-spacing: 0.05em;
		position: relative;
		overflow: hidden;
	}

	.vintage-button::after {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: linear-gradient(rgba(255,255,255,0.1), rgba(255,255,255,0));
		pointer-events: none;
	}

	.vintage-button:hover {
		transform: translateY(-1px);
	}

	.vintage-button:active {
		transform: translateY(1px);
		box-shadow: 0 0 1px rgba(0, 0, 0, 0.3);
	}

	.vintage-button-primary {
		background-color: #d6b171;
	}

	.vintage-button-secondary {
		background-color: #a7b5a2;
	}

	.vintage-button-tertiary {
		background-color: #b8c0cc;
	}

	.vintage-button-danger {
		background-color: #c9a0a0;
	}

	.vintage-checkbox {
		-webkit-appearance: none;
		appearance: none;
		width: 16px;
		height: 16px;
		border: 1px solid var(--sepia);
		border-radius: 2px;
		background-color: var(--cream);
		display: inline-block;
		position: relative;
		margin: 0;
		cursor: pointer;
	}

	.vintage-checkbox:checked::after {
		content: '✓';
		position: absolute;
		color: var(--sepia);
		font-size: 14px;
		top: -2px;
		left: 2px;
	}

	.vintage-toggle-button {
		background: none;
		border: none;
		color: var(--sepia);
		font-size: 0.9rem;
		text-decoration: underline;
		cursor: pointer;
		transition: color 0.3s;
	}

	.vintage-toggle-button:hover {
		color: #906734;
	}

	.vintage-instruction {
		font-style: italic;
		opacity: 0.85;
	}

	.vintage-texture {
		background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3CfeColorMatrix type='matrix' values='1 0 0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 0.5 0' /%3E%3C/filter%3E%3Crect width='100' height='100' filter='url(%23noise)' /%3E%3C/svg%3E");
		filter: contrast(170%) brightness(800%);
		mix-blend-mode: multiply;
	}

	.vintage-color-input {
		border: 1px solid var(--sepia);
		border-radius: 3px;
		width: 32px;
		cursor: pointer;
		background-color: transparent;
	}
	
	/* Make grid more responsive on small screens */
	@media (max-width: 640px) {
		.grid {
			gap: 8px;
		}
	}
</style>
