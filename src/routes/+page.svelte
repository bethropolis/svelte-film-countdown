<script>
	import FilmCountdown from '$lib/FilmCountdown.svelte';

	// --- Reactive State ---
	let initialCount = $state(5);
	let countdownDuration = $state(1000);
	let loop = $state(false);
	let numberColor = $state('#4A3B4A'); // Deep purplish-brown
	let numCircles = $state(4);

	let countdownInstance;

	// --- Event Handlers ---
	function handleComplete() {
		console.log('Countdown finished!');
	}

	function handleEachCount(currentCount) {
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
	<header class="p-6 flex justify-end bg-gray-100 border-b border-gray-300">
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
		<div class="relative transform scale-125 md:scale-150 transition-transform duration-300 w-full max-w-lg">
			<FilmCountdown
				bind:this={countdownInstance}
				initialCount={initialCount}
				countdownDuration={countdownDuration}
				loop={loop}
				onComplete={handleComplete}
				onEachCount={handleEachCount}
				config={{ numberColor: numberColor, numCircles: numCircles }}
			/>
		</div>
	</main>

	<!-- Footer - Controls -->
	<footer class="p-6 bg-white shadow-lg rounded-t-lg mt-auto border-t border-gray-300">
		<div class="flex flex-wrap justify-center items-center gap-4">
			<!-- Count Control -->
			<div class="flex items-center">
				<label for="initialCount" class="control-label">Count:</label>
				<input type="number" id="initialCount" bind:value={initialCount} class="control-input focus:outline-none focus:shadow-outline focus:border-indigo-500" />
			</div>

			<!-- Duration Control -->
			<div class="flex items-center">
				<label for="countdownDuration" class="control-label">Duration (ms):</label>
				<input
					type="number"
					id="countdownDuration"
					bind:value={countdownDuration}
					class="control-input focus:outline-none focus:shadow-outline focus:border-indigo-500"
				/>
			</div>

			<!-- Color Control -->
			<div class="flex items-center">
				<label for="numberColor" class="control-label">Color:</label>
				<input
					type="color"
					id="numberColor"
					bind:value={numberColor}
					class="control-input-color focus:shadow-outline focus:border-indigo-500"
				/>
			</div>

			<!-- Circles Control -->
			<div class="flex items-center">
				<label for="numCircles" class="control-label">Circles:</label>
				<input type="number" id="numCircles" bind:value={numCircles} class="control-input" />
			</div>

			<!-- Loop Control -->
			<div class="flex items-center">
				<input type="checkbox" id="loop" bind:checked={loop} class="control-checkbox mr-1 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-indigo-500" />
				<label for="loop" class="control-label">Loop</label>
			</div>

			<!-- Action Buttons -->
			<div>
				<button on:click={startCountdown} class="control-button bg-emerald-600 hover-bg-emerald-700">
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
	</footer>
</div>

<style>
	/* --- Control Label --- */
	.control-label {
		@apply text-sm text-gray-700 font-medium mr-2;
	}

	/* --- Control Input (Text & Number) --- */
	.control-input {
		@apply shadow appearance-none border rounded w-20 py-1 px-2 text-gray-700 leading-tight border-gray-400 text-sm ;
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
		@apply h-4 w-4 text-indigo-600 border-gray-300 rounded ;
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