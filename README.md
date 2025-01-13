# svelte-film-countdown

A vintage-style film countdown component for your Svelte 5 applications, built with runes.

![Example Countdown](static/image.png)

## Features

*   **Authentic Vintage Aesthetic:**  Provides a visually compelling retro film countdown experience reminiscent of classic cinema.
*   **Highly Customizable:**  Tailor the countdown's behavior and appearance with props and CSS variables. Control duration, initial count, styling of circles, numbers, and visual effects.
*   **Programmatic Control:**  Imperatively manage the countdown's state using exported functions: `start`, `pause`, `resume`, and `reset`.
*   **Event Callbacks:**  Execute custom logic when the countdown completes (`onComplete`) or after each decrement (`onEachCount`).
*   **Looping Functionality:**  Enable continuous countdown loops with the `loop` prop.
*   **CSS Variable Theming:**  Achieve consistent branding and easily adjust the visual theme using CSS variables.

## Installation

Install `svelte-film-countdown` using your preferred package manager:

```bash
npm install svelte-film-countdown
```

```bash
yarn add svelte-film-countdown
```

```bash
pnpm add svelte-film-countdown
```

```bash
bun add svelte-film-countdown
```

## Usage

Import the `FilmCountdown` component into your Svelte component:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';

  let countdownInstance;

  function handleComplete() {
    alert('Countdown finished!');
  }

  function handleEachCount(currentCount) {
    console.log('Current count:', currentCount);
  }

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

<FilmCountdown
  bind:this={countdownInstance}
  initialCount={5}
  countdownDuration={1000}
  onComplete={handleComplete}
  onEachCount={handleEachCount}
/>

<br />
<button on:click={startCountdown}>Start</button>
<button on:click={pauseCountdown}>Pause</button>
<button on:click={resumeCountdown}>Resume</button>
<button on:click={resetCountdown}>Reset</button>
```

### Basic Example

Render the countdown with default settings:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';
</script>

<FilmCountdown />
```

### Custom Initial Count and Duration

Configure the starting number and the duration of each count:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';
</script>

<FilmCountdown initialCount={10} countdownDuration={500} />
```

### Utilizing Event Callbacks

Execute functions when the countdown finishes or after each count:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';

  function handleComplete() {
    console.log('Countdown finished!');
  }

  function handleEachCount(currentCount) {
    console.log('Current count:', currentCount);
  }
</script>

<FilmCountdown onComplete={handleComplete} onEachCount={handleEachCount} />
```

### Configuration Options via `config` Prop

Customize the visual appearance of the countdown through the `config` prop:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';
</script>

<FilmCountdown
  initialCount={3}
  config={{
    numberColor: "red",
    numCircles: 4
  }}
/>
```

### Enabling Looping

Make the countdown restart automatically after reaching zero. Note that you need to manually trigger `start()` to begin the countdown:

```svelte
<script>
  import FilmCountdown from 'svelte-film-countdown';
  let countdownInstance;

  function startLoopingCountdown() {
    countdownInstance?.start();
  }
</script>

<FilmCountdown bind:this={countdownInstance} loop={true} />
<button on:click={startLoopingCountdown}>Start Looping Countdown</button>
```

## Props

| Prop Name          | Type                          | Default Value | Description                                                                                                       |
| ------------------ | ----------------------------- | ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `initialCount`     | `number`                      | `5`           | The number from which the countdown begins.                                                                       |
| `countdownDuration`| `number`                      | `1000`        | The time in milliseconds for each count decrement.                                                                |
| `onComplete`       | `() => void`                  | `() => {}`    | A callback function that is executed when the countdown reaches 0.                                               |
| `onEachCount`      | `(count: number) => void`     | `() => {}`    | A callback function executed after each count, receiving the current count as an argument.                      |
| `config`           | `object`                      | `{}`          | An object containing properties to customize the visual aspects of the countdown. See the `config` details below. |
| `loop`             | `boolean`                     | `false`       | If `true`, the countdown will automatically reset to `initialCount` and stop at 0, ready to be started again.   |

### `config` Object Properties

| Property Name           | Type     | Default Value                           | Description                                                                                                       |
| ----------------------- | -------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `numberColor`           | `string` | `'var(--countdown-number-color)'`         | The color of the number displayed in the center of the countdown.                                                 |
| `numCircles`            | `number` | `2`                                       | The number of concentric circles animating around the central number.                                              |
| `circleRadius`          | `number` | `64`                                      | The radius of the outermost circle in pixels.                                                                     |
| `circleSpacing`         | `number` | `8`                                       | The space in pixels between each concentric circle.                                                               |
| `strokeRatio`           | `number` | `1 / 40`                                  | The ratio of the circle's radius that determines the thickness of the circle's stroke.                             |
| `canvasSize`            | `number` | `256`                                     | The width and height of the SVG canvas (viewBox dimensions). The component will maintain a 16:9 aspect ratio.      |
| `circleGradientStart`   | `string` | `'var(--circle-gradient-start)'`          | The starting color of the gradient applied to the circles.                                                        |
| `circleGradientMiddle`  | `string` | `'var(--circle-gradient-middle)'`         | The middle color of the gradient applied to the circles.                                                          |
| `circleGradientMiddleOpacity` | `number` | `0.8`                                 | The opacity of the middle color in the circle gradient.                                                           |
| `circleGradientEnd`     | `string` | `'var(--circle-gradient-end)'`            | The ending color of the gradient applied to the circles.                                                          |

## Exported Functions

To control the countdown programmatically, you need to bind a reference to the `FilmCountdown` component using `bind:this`.

*   **`start()`:** Initiates the countdown sequence.
*   **`pause()`:**  Temporarily halts the countdown.
*   **`resume()`:** Continues the countdown from the paused state.
*   **`reset()`:**  Stops the countdown and resets it to the `initialCount`.

## Theming with CSS Variables

Customize the visual theme of the `FilmCountdown` component by overriding these CSS variables in your global stylesheet or within a specific component's `<style>` block.

```css
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

  --grid-line-color: rgba(255, 255, 255, 0.3);
  --film-grain-opacity: 0.1;
  --flicker-duration: 0.15s;
  --vignette-strength: 0.4;
  --center-vignette-strength: 0.15;
  --scratch-color: rgba(255, 255, 255, 0.2);
  --scratch-opacity: 0.15;
  --number-font: monospace;
  --number-text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
  --film-grain-pattern: linear-gradient(to bottom, rgba(156, 163, 175, 0.05), transparent, rgba(156, 163, 175, 0.05));
}
```

**Example of Applying a Theme:**

```svelte
<style>
  :root {
    --countdown-number-color: lightyellow;
    --sweeping-background-color: rgba(100, 0, 0, 0.25);
    --film-grain-opacity: 0.2;
  }
</style>

<FilmCountdown />
```

## Contributing

Contributions are highly appreciated! If you find a bug, have an idea for an improvement, or want to add a new feature, please don't hesitate to open an issue or submit a pull request.

## License

[MIT License](LICENSE)
