<script lang="ts">
	import P5Sketch, { type Sketch } from '$lib/index.js';

	let diameter = $state(100);

	const sketch: Sketch = (p5) => {
		p5.setup = () => {
			p5.createCanvas(400, 400);
		};
		p5.draw = () => {
			p5.background(200);
			p5.circle(p5.width / 2, p5.height / 2, diameter);
		};
	};
</script>

<P5Sketch {sketch} />

<label>
	diameter
	<input type="range" bind:value={diameter} min={0} max={400} />
	{diameter}
</label>
