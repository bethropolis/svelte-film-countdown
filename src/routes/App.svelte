<script>
  import FilmCountdown from '$lib/FilmCountdown.svelte';
  import blooperSound from '../asserts/Blooper.mp3'

  let countdownInstance1;
  let countdownInstance2;
  let countdownInstance3;
  let countdownInstance4;
  let countdownInstanceWithSound;

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

function playBleep() {
  const audio = new Audio(blooperSound);
  audio.volume = 0.1;
  audio.play();
}
</script>

<main>
  <div>
    <FilmCountdown
      bind:this={countdownInstance1}
      onComplete={handleComplete}
      onEachCount={handleEachCount}
    >
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

  <br>

  <div>
    <h2>Countdown with Sound</h2>
    <FilmCountdown
      bind:this={countdownInstanceWithSound}
      initialCount={5}
      onEachCount={playBleep}
    >
      <span slot="branding">Sound Effects</span>
    </FilmCountdown>
    <br />
    <button onclick={() => startExternalCountdown(countdownInstanceWithSound)}>Start</button>
    <button onclick={() => pauseExternalCountdown(countdownInstanceWithSound)}>Pause</button>
    <button onclick={() => resumeExternalCountdown(countdownInstanceWithSound)}>Resume</button>
    <button onclick={() => resetExternalCountdown(countdownInstanceWithSound)}>Reset</button>
  </div>
</main>

<style>

</style>
