<script lang="ts">
	import P5Sketch, { type Sketch } from '$lib/index.js';

	let x = 0;
	let y = 0;
	let diameter = $state(100);

	const sketch: Sketch = (p) => {
		p.setup = () => {
			p.createCanvas(800, 600);
			p.noStroke();
		};
		p.draw = () => {
			p.background(10);
			x = p.lerp(x, p.mouseX, 0.05);
			y = p.lerp(y, p.mouseY, 0.05);
			p.fill(255);
			p.circle(x, y, diameter);
		};
	};
</script>

<P5Sketch {sketch} />

<label>
	diameter
	<input type="range" bind:value={diameter} min={0} max={400} />
	{diameter}
</label>
