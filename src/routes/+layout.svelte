<script>
    import "../app.css";
    import Github from './github.svelte';
</script>

<svelte:head>
  <!-- Adding vintage fonts from Google Fonts -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Special+Elite&family=Abril+Fatface&display=swap" rel="stylesheet">
</svelte:head>

  <div class="min-h-screen bg-cream flex flex-col">
    <!-- Header -->
    <header class="bg-cream p-6 w-full flex justify-between items-center">
      <h1 class="text-xl font-semibold vintage-title select-none">Vintage Film Countdown</h1>
      <a
        href="https://github.com/bethropolis/svelte-film-countdown"
        target="_blank"
        rel="noopener noreferrer"
        title="github repository"
        class="text-gray-600 hover:text-indigo-300 font-medium text-sm"
      >
        <Github class="w-7 h-7" />
      </a>
    </header>
  <slot />
</div>

	
<style>
	/* Define your vintage color palette */
	:root {
		--cream: #f8f0e3;
		--vintage-font: 'Special Elite', cursive;
		--vintage-title-font: 'Abril Fatface', serif;
		--sepia: #704214;
		--aged-paper: #d2b48c;
	}

	.bg-cream {
		background-color: var(--cream);
	}

	:global(body) {
		font-family: var(--vintage-font);
		letter-spacing: 0.03em;
		line-height: 1.6;
		color: var(--sepia);
		background-color: var(--cream);
	}

	:global(button, input) {
		font-family: var(--vintage-font);
	}

	.vintage-title {
		font-family: var(--vintage-title-font);
		letter-spacing: 0.05em;
		text-shadow: 1px 1px 0px rgba(0,0,0,0.05);
		color: var(--sepia);
	}
	
	/* Add a subtle vignette effect to the page */
	:global(.min-h-screen) {
		position: relative;
		box-shadow: inset 0 0 100px rgba(0,0,0,0.1);
	}
</style>
