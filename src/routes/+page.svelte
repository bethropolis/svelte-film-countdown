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
		canvasSize: 256 
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

<div class="min-h-screen bg-gray-200 flex flex-col">
	<!-- Header -->
	<header class="p-6 flex justify-end border-">
		<a
			href="YOUR_GITHUB_REPOSITORY_LINK_HERE"
			target="_blank"
			rel="noopener noreferrer"
			class="text-indigo-700 hover:text-indigo-900 font-medium text-sm"
		>
			GitHub
		</a>
	</header>

	<!-- Main Content -->
	<main class="flex-grow flex flex-col items-center justify-center p-8">
		<!-- Film Countdown Component -->
		<div
			class="relative transform scale-125 md:scale-150 transition-transform duration-300 w-full max-w-lg"
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
	<footer class="p-6 bg-white shadow-lg rounded-t-lg mt-auto border-t border-gray-300">
		<div class="flex flex-col items-center justify-center gap-4 mb-4">
			<!-- Crucial Controls -->
			<div class="flex flex-wrap justify-center items-center gap-4">
				<div class="flex items-center">
					<label for="initialCount" class="control-label">Count:</label>
					<input
						type="number"
						id="initialCount"
						bind:value={initialCount}
						class="control-input focus:outline-none focus:shadow-outline focus:border-indigo-500"
					/>
				</div>

				<div class="flex items-center">
					<label for="countdownDuration" class="control-label">Duration (ms):</label>
					<input
						type="number"
						id="countdownDuration"
						bind:value={countdownDuration}
						class="control-input focus:outline-none focus:shadow-outline focus:border-indigo-500"
					/>
				</div>
			</div>

			<!-- Action Buttons -->
			<div class="flex flex-wrap justify-center items-center gap-4">
				<button
					on:click={startCountdown}
					class="control-button bg-emerald-600 hover-bg-emerald-700"
				>
					Start
				</button>
				<button on:click={pauseCountdown} class="control-button bg-amber-600 hover-bg-amber-700">
					Pause
				</button>
				<button on:click={resumeCountdown} class="control-button bg-sky-600 hover-bg-sky-700">
					Resume
				</button>
				<button on:click={resetCountdown} class="control-button bg-rose-600 hover-bg-rose-700">
					Reset
				</button>
			</div>
		</div>

		<!-- Advanced Options Toggle -->
		<div class="text-center mb-4">
			<button
				type="button"
				class="text-sm text-gray-600 hover:text-gray-800 focus:outline-none"
				on:click={() => (showAdvancedOptions = !showAdvancedOptions)}
			>
				{showAdvancedOptions ? 'Hide Advanced Options' : 'Show Advanced Options'}
			</button>
		</div>

		<!-- Advanced Options -->
		{#if showAdvancedOptions}
			<div class="flex flex-wrap justify-center items-center gap-4">
				<!-- Color Control -->
				<div class="flex items-center">
					<label for="numberColor" class="control-label">Color:</label>
					<input
						type="color"
						id="numberColor"
						bind:value={config.numberColor}
						class="control-input-color focus:shadow-outline focus:border-indigo-500"
					/>
				</div>

				<!-- Circles Control -->
				<div class="flex items-center">
					<label for="numCircles" class="control-label">Circles:</label>
					<input type="number" id="numCircles" bind:value={config.numCircles} class="control-input" />
				</div>

				<!-- Radius Control -->
				<div class="flex items-center">
					<label for="circleRadius" class="control-label">Radius:</label>
					<input
						type="number"
						id="circleRadius"
						bind:value={config.circleRadius}
						class="control-input"
					/>
				</div>

				<!-- Stroke Control -->
				<div class="flex items-center">
					<label for="strokeRatio" class="control-label">Stroke:</label>
					<input
						type="number"
						id="strokeRatio"
						bind:value={config.strokeRatio}
						step="0.01"
						class="control-input"
					/>
				</div>

				<!-- Spacing Control -->
				<div class="flex items-center">
					<label for="circleSpacing" class="control-label">Spacing:</label>
					<input
						type="number"
						id="circleSpacing"
						bind:value={config.circleSpacing}
						class="control-input"
					/>
				</div>

				<!-- Loop Control -->
				<div class="flex items-center">
					<input
						type="checkbox"
						id="loop"
						bind:checked={loop}
						class="control-checkbox mr-1 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-indigo-500"
					/>
					<label for="loop" class="control-label">Loop</label>
				</div>

				<!-- Beep Control -->
				<div class="flex items-center">
					<input
						type="checkbox"
						id="beep"
						bind:checked={beep}
						class="control-checkbox mr-1 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-indigo-500"
					/>
					<label for="beep" class="control-label">Beep</label>
				</div>
			</div>
		{/if}
	</footer>
</div>

<style>
	/* --- Control Label --- */
	.control-label {
		@apply text-sm text-gray-700 font-medium mr-2;
	}

	/* --- Control Input (Text & Number) --- */
	.control-input {
		@apply shadow appearance-none border rounded w-20 py-1 px-2 text-gray-700 leading-tight border-gray-400 text-sm;
	}

	/* --- Control Input (Color) --- */
	.control-input-color {
		@apply w-24 h-8 border rounded-md border-gray-400;
	}

	.control-input-color:focus {
		@apply outline-none;
	}

	/* --- Control Checkbox --- */
	.control-checkbox {
		@apply h-4 w-4 text-indigo-600 border-gray-300 rounded;
	}

	/* --- Control Button --- */
	.control-button {
		@apply text-white font-bold py-1 px-3 rounded shadow-sm border border-transparent;
	}

	.control-button:hover {
		@apply bg-opacity-90;
	}

	.control-button:active {
		@apply bg-opacity-75;
	}

	/* --- Hover Background Colors (Specific) --- */
	.bg-emerald-600.hover-bg-emerald-700:hover {
		background-color: theme('colors.emerald.700');
	}

	.bg-amber-600.hover-bg-amber-700:hover {
		background-color: theme('colors.amber.700');
	}

	.bg-sky-600.hover-bg-sky-700:hover {
		background-color: theme('colors.sky.700');
	}

	.bg-rose-600.hover-bg-rose-700:hover {
		background-color: theme('colors.rose.700');
	}
</style>