<script>
    import "../app.css";
    import Github from './github.svelte';
</script>


  <div class="min-h-screen bg-cream flex flex-col">
    <!-- Header -->
    <header class="bg-cream p-6 w-full flex justify-between items-center">
      <h1 class="text-xl font-semibold text-gray-800">Vintage Film Countdown</h1>
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
	}

	.bg-cream {
		background-color: var(--cream);
	}
</style>
