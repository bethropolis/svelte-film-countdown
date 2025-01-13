<script>
	import FilmCountdown from '$lib/FilmCountdown.svelte';
	import Blooper from '../asserts/Blooper.mp3';
	import Github from './github.svelte';

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

<div class="min-h-screen bg-cream flex flex-col">
	<!-- Header -->
	<header class="bg-cream p-6 w-full flex justify-between items-center">
		<h1 class="text-xl font-semibold text-gray-800">Vintage Film Countdown</h1>
		<a
			href="YOUR_GITHUB_REPOSITORY_LINK_HERE"
			target="_blank"
			rel="noopener noreferrer"
			title="github repository"
			class="text-gray-600 hover:text-indigo-300 font-medium text-sm"
		>
			<Github class="w-7 h-7"/>
		</a>
	</header>

	<!-- Main Content -->
	<main class="p-8 w-full flex flex-col items-center flex-grow justify-center">
		<!-- Film Countdown Component -->
		<div
			class="relative w-[320px] md:w-[400px] lg:w-[512px] transform scale-150 md:scale-175 lg:scale-200 transition-transform duration-300"
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
	<footer class="bg-white p-2 w-full border-t border-gray-300 shadow-inner">
		<p class="text-sm text-gray-600 text-center mb-6">
			Adjust the settings to see the countdown change.
		</p>

		<!-- Basic Settings in Single Line -->
		<!-- Basic Settings with Vintage Colors and Darker Text -->
		<div class="flex items-center justify-center gap-6 mb-4 flex-wrap">
			<div class="flex items-center">
				<label
					for="initialCount"
					class="text-sm font-medium text-gray-700 mr-2"
					title="Starting number"
				>
					Count:
				</label>
				<input
					type="number"
					id="initialCount"
					bind:value={initialCount}
					class="shadow border rounded w-20 py-1 px-2 text-gray-800 text-sm focus:outline-none focus:ring-2 focus:ring-amber-600"
				/>
			</div>

			<div class="flex items-center">
				<label
					for="countdownDuration"
					class="text-sm font-medium text-gray-700 mr-2"
					title="Time in milliseconds for each count"
				>
					Duration (ms):
				</label>
				<input
					type="number"
					id="countdownDuration"
					bind:value={countdownDuration}
					class="shadow border rounded w-20 py-1 px-2 text-gray-800 text-sm focus:outline-none focus:ring-2 focus:ring-amber-600"
				/>
			</div>

			<!-- Buttons with Vintage Colors and Darker Text -->
			<div class="flex gap-4">
				<button
					onclick={startCountdown}
					class="bg-amber-300 hover:bg-amber-400 text-gray-900 font-bold py-1 px-4 rounded focus:outline-none focus:ring-2 focus:ring-amber-500 shadow-sm text-sm"
				>
					Start
				</button>
				<button
					onclick={pauseCountdown}
					class="bg-violet-300 hover:bg-olive-500 text-gray-900 font-bold py-1 px-4 rounded focus:outline-none focus:ring-2 focus:ring-olive-500 shadow-sm text-sm"
				>
					Pause
				</button>
				<button
					onclick={resumeCountdown}
					class="bg-teal-300 hover:bg-teal-400 text-gray-900 font-bold py-1 px-4 rounded focus:outline-none focus:ring-2 focus:ring-teal-500 shadow-sm text-sm"
				>
					Resume
				</button>
				<button
					onclick={resetCountdown}
					class="bg-rose-300 hover:bg-crimson-400 text-gray-900 font-bold py-1 px-4 rounded focus:outline-none focus:ring-2 focus:ring-crimson-500 shadow-sm text-sm"
				>
					Reset
				</button>
			</div>
		</div>

		<!-- Advanced Options Toggle -->
		<div class="text-center mb-4">
			<button
				type="button"
				class="text-sm text-gray-600 hover:text-gray-800 focus:outline-none"
				onclick={() => (showAdvancedOptions = !showAdvancedOptions)}
			>
				{showAdvancedOptions ? 'Hide Advanced Options' : 'Show Advanced Options'}
			</button>
		</div>

		{#if showAdvancedOptions}
			<div class="border-t border-gray-300 pt-4">
				<div class="flex flex-wrap items-center justify-center gap-4">
					<div class="flex items-center">
						<label
							for="numberColor"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Color of the countdown number">Color:</label
						>
						<input type="color" id="numberColor" bind:value={config.numberColor} class="h-8" />
					</div>
					<div class="flex items-center">
						<label
							for="numCircles"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Number of concentric circles">Circles:</label
						>
						<input
							type="number"
							id="numCircles"
							bind:value={config.numCircles}
							class="shadow border rounded w-16 py-1 px-2 text-gray-700 text-sm"
						/>
					</div>
					<div class="flex items-center">
						<label
							for="circleRadius"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Radius of the largest circle">Radius:</label
						>
						<input
							type="number"
							id="circleRadius"
							bind:value={config.circleRadius}
							class="shadow border rounded w-16 py-1 px-2 text-gray-700 text-sm"
						/>
					</div>
					<div class="flex items-center">
						<label
							for="strokeRatio"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Thickness of the circle lines">Stroke:</label
						>
						<input
							type="number"
							id="strokeRatio"
							bind:value={config.strokeRatio}
							step="0.01"
							class="shadow border rounded w-16 py-1 px-2 text-gray-700 text-sm"
						/>
					</div>
					<div class="flex items-center">
						<label
							for="circleSpacing"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Space between the circles">Spacing:</label
						>
						<input
							type="number"
							id="circleSpacing"
							bind:value={config.circleSpacing}
							class="shadow border rounded w-16 py-1 px-2 text-gray-700 text-sm"
						/>
					</div>
					<div class="flex items-center">
						<label
							for="canvasSize"
							class="text-sm font-medium text-gray-700 mr-2"
							title="Overall size of the countdown SVG">Canvas Size:</label
						>
						<input
							type="number"
							id="canvasSize"
							bind:value={config.canvasSize}
							class="shadow border rounded w-16 py-1 px-2 text-gray-700 text-sm"
						/>
					</div>
					<div class="flex items-center">
						<input
							type="checkbox"
							id="loop"
							bind:checked={loop}
							class="h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
						/>
						<label for="loop" class="ml-2 text-sm font-medium text-gray-700">Loop</label>
					</div>
					<div class="flex items-center">
						<input
							type="checkbox"
							id="beep"
							bind:checked={beep}
							class="h-4 w-4 text-indigo-600 border-gray-300 rounded focus:ring-indigo-500"
						/>
						<label for="beep" class="ml-2 text-sm font-medium text-gray-700">Beep</label>
					</div>
				</div>
			</div>
		{/if}
	</footer>
</div>

<style>
	/* Define your vintage color palette */
	:root {
		--cream: #f8f0e3;
	}

	.bg-cream {
		background-color: var(--cream);
	}
</style>
