<script lang="ts">
	import P5Sketch, { type Sketch } from '$lib/index.js';

	let x = 0;
	let y = 0;
	let diameter = $state(100);

	const sketch: Sketch = ($) => {
		$.setup = () => {
			$.createCanvas(800, 600);
			$.noStroke();
		};
		$.draw = () => {
			$.background(10);
			x = $.lerp(x, $.mouseX, 0.05);
			y = $.lerp(y, $.mouseY, 0.05);
			$.fill(255);
			$.circle(x, y, diameter);
		};
	};
</script>

<P5Sketch {sketch} />

<label>
	diameter
	<input type="range" bind:value={diameter} min={0} max={400} />
	{diameter}
</label>
