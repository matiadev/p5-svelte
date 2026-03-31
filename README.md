<p align="center">
	<img src="./static/banner.png" alt="@sveltecraft/p5-svelte" width="800" height="400">
</p>

# @sveltecraft/p5-svelte

A Svelte component wrapper for p5.js sketches.

## Installation

```sh
npm i @sveltecraft/p5-svelte p5
```

## Usage

> ⚠️ **Warning**
> p5.js 2.0+ has a slightly different API than older versions. In the new version, asset preloading is done in `setup` instead of the `preload` function.

Import the `P5Sketch` component and the optional `Sketch` type. After that create a `sketch` function with a `setup` and `draw` function. Since we're using [instance mode](https://github.com/processing/p5.js/wiki/Global-and-instance-mode) we have to prefix any p5 method with `p` which we receive as an argument:

```svelte
<script lang="ts">
	import P5Sketch, { type Sketch } from '@sveltecraft/p5-svelte'

	let x = 0
	let y = 0
	let diameter = $state(100)

	const sketch: Sketch = (p) => {
		p.setup = () => {
			p.createCanvas(800, 600)
			p.noStroke()
		}
		p.draw = () => {
			p.background(4)
			x = p.lerp(x, p.mouseX, 0.05)
			y = p.lerp(y, p.mouseY, 0.05)
			p.fill(255)
			p.circle(x, y, diameter)
		}
	}
</script>

<P5Sketch {sketch} />

<label>
	diameter
	<input type="range" bind:value={diameter} min={0} max={400} />
	{diameter}
</label>
```

## Multiple Sketches

You can create as many sketches as you want:

```svelte
<script lang="ts">
	import P5Sketch, { type Sketch } from '@sveltecraft/p5-svelte'

	const walker: Sketch = (p) => {
		// ...
	}

	const randomNumberDistribution: Sketch = (p) => {
		// ...
	}
</script>

<!-- walker -->
<P5Sketch sketch={walker} />

<!-- random number distribution -->
<P5Sketch sketch={randomNumberDistribution} />
```

## Abstracting Code

If you want to abstract your code into a class or function keep in mind you have to pass the p5 object to it:

```svelte
<script lang="ts">
	import type p5 from 'p5'

	class Walker {
		p: p5
		x = 0
		y = 0

		constructor(p: p5) {
			this.p = p
			this.x = this.p.width / 2
			this.y = this.p.height / 2
		}

		show() {
			this.p.stroke(255)
			this.p.point(this.x, this.y)
		}

		step() {
			let xstep = this.p.random(-1, 1)
			let ystep = this.p.random(-1, 1)
			this.x += xstep
			this.y += ystep
		}
	}

	const sketch: Sketch = (p) => {
		let walker: Walker
		p.setup = () => {
			walker = new Walker(p)
			p.createCanvas(400, 400)
			p.background(4)
		}
		p.draw = () => {
			walker.step()
			walker.show()
		}
	}
</script>

<P5Sketch {sketch} />
```

## Addons

This section shows how to use legacy addons that only work with a global p5 instance and ones that support instance mode. You have to install addons using [npm](https://npmx.dev/) instead of using a CDN.

### Legacy Global Mode Addons

Addons like `p5.sound` work only in global mode, so we have to load them using the `addons` array before the sketch initializes:

```svelte
<script lang="ts">
	import P5Sketch, { type Sketch } from '@sveltecraft/p5-svelte'

	// 🚫 this doesn't work
	import 'p5.sound'

	// 👍 pass your legacy addons here
	const addons = [() => import('p5.sound')]

	const sketch: Sketch = (p) => {
		let sound: any
		p.setup = async () => {
			sound = await (p as any).loadSound('/sfx.mp3')
			p.createCanvas(400, 400)
		}
		p.draw = () => {
			p.background(10)
		}
		p.mousePressed = () => {
			sound.play()
		}
	}
</script>

<!-- pass the addons -->
<P5Sketch {sketch} {addons} />
```

Addons like `p5.sound` look for `window.p5` at load time to extend the library. By passing addons as an array, the component:

1. Loads `p5` dynamically
2. Assigns `p5` to `window.p5` before loading addons
3. Waits for all addons to load
4. Then initializes your sketch

This ensures addons are properly registered before your sketch runs.

### Instance Mode Addons

Addons like `p5.brush` support global and instance mode. You can learn how to use it by reading their docs. Here's an example of using the `p5.brush` addon:

```svelte
<script lang="ts">
	import P5Sketch, { type Sketch } from '@sveltecraft/p5-svelte'
	import * as brush from 'p5.brush'

	const sketch: Sketch = (p) => {
		brush.instance(p)

		p.setup = () => {
			p.createCanvas(400, 400, p.WEBGL)
		}

		p.draw = () => {
			// ...
		}
	}
</script>

<P5Sketch {sketch} />
```

## API

### Props

- `sketch` (required): A function that receives a p5 instance and defines `setup` and `draw` methods
- `addons` (optional): Array of async functions that load p5 addons
- `style` (optional): CSS styles to apply to the canvas container
- `class` (optional): CSS class to apply to the canvas container
