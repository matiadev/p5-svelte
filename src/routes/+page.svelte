<script lang="ts">
	import Canvas, { type Sketch } from '$lib/index.js';

	let diameter = $state(100);

	// @ts-expect-error cursed library
	// const addons = [() => import('p5.sound')];

	// import type p5 from 'p5';
	// interface SoundMethods {
	// 	loadSound: (path: string) => Promise<{
	// 		play: () => void;
	// 		stop: () => void;
	// 		isPlaying: () => boolean;
	// 	}>;
	// }

	const sketch: Sketch = (s) => {
		// const p = s as p5 & SoundMethods;
		// let sound: Awaited<ReturnType<SoundMethods['loadSound']>>;
		s.setup = async () => {
			// sound = await p.loadSound('./sfx.mp3');
			s.createCanvas(400, 400);
		};
		s.draw = () => {
			s.background(200);
			s.circle(s.width / 2, s.height / 2, diameter);
		};
		// s.mousePressed = () => {
		// 	return sound.isPlaying() ? sound.stop() : sound.play();
		// };
	};
</script>

<Canvas {sketch} />

<label>
	diameter
	<input type="range" bind:value={diameter} min={0} max={400} />
	{diameter}
</label>
