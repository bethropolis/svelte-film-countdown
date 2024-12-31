<script>
    import FilmCountdown from '$lib/FilmCountdown.svelte';
  
    let countdownInstance1;
    let countdownInstance2;
    let countdownInstance3;
    let countdownInstance4;
  
    function handleComplete() {
      alert('Countdown finished!');
    }
  
    function handleEachCount(currentCount) {
      console.log('Current count:', currentCount);
    }
  
    function startExternalCountdown(instance) {
      instance?.start();
    }
  
    function pauseExternalCountdown(instance) {
      instance?.pause();
    }
  
    function resumeExternalCountdown(instance) {
      instance?.resume();
    }
  
    function resetExternalCountdown(instance) {
      instance?.reset();
    }
  </script>
  
  <main>
    <div>
      <FilmCountdown bind:this={countdownInstance1} onComplete={handleComplete} onEachCount={handleEachCount}>
        <span slot="branding">Vintage Inc.</span>
      </FilmCountdown>
      <br />
      <button onclick={() => startExternalCountdown(countdownInstance1)}>Start</button>
      <button onclick={() => pauseExternalCountdown(countdownInstance1)}>Pause</button>
      <button onclick={() => resumeExternalCountdown(countdownInstance1)}>Resume</button>
      <button onclick={() => resetExternalCountdown(countdownInstance1)}>Reset</button>
    </div>
  
    <br />
  
    <div>
      <FilmCountdown bind:this={countdownInstance2} initialCount={10}>
        <span slot="branding">Acme Corp</span>
      </FilmCountdown>
      <br />
      <button onclick={() => startExternalCountdown(countdownInstance2)}>Start</button>
      <button onclick={() => pauseExternalCountdown(countdownInstance2)}>Pause</button>
      <button onclick={() => resumeExternalCountdown(countdownInstance2)}>Resume</button>
      <button onclick={() => resetExternalCountdown(countdownInstance2)}>Reset</button>
    </div>
  
    <br />
  
    <div>
      <FilmCountdown
        bind:this={countdownInstance3}
        initialCount={3}
        countdownDuration={500}
        config={{
          startLabel: "Begin!",
          countingLabel: "Ticking..."
        }}
        loop={true}
      >
        <span slot="branding">Global Industries</span>
      </FilmCountdown>
      <br />
      <button onclick={() => startExternalCountdown(countdownInstance3)}>Start</button>
      <button onclick={() => pauseExternalCountdown(countdownInstance3)}>Pause</button>
      <button onclick={() => resumeExternalCountdown(countdownInstance3)}>Resume</button>
      <button onclick={() => resetExternalCountdown(countdownInstance3)}>Reset</button>
    </div>
  
    <br>
  
    <div>
      <FilmCountdown
        bind:this={countdownInstance4}
        initialCount={5}
        onComplete={handleComplete}
        onEachCount={handleEachCount}
        config={{
          numberColor: "red",
          buttonColor: "blue",
          buttonTextColor: "white",
          numCircles: 5
        }}
      >
      </FilmCountdown>
      <br />
      <button onclick={() => startExternalCountdown(countdownInstance4)}>Start</button>
      <button onclick={() => pauseExternalCountdown(countdownInstance4)}>Pause</button>
      <button onclick={() => resumeExternalCountdown(countdownInstance4)}>Resume</button>
      <button onclick={() => resetExternalCountdown(countdownInstance4)}>Reset</button>
    </div>
  </main>
  
  <style>
  
  </style>