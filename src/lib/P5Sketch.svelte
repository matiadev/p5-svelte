<script lang="ts">
	import type p5 from 'p5';
	import type { Sketch } from './types.js';

	let props: {
		sketch: Sketch;
		addons?: Array<() => Promise<unknown>>;
		style?: string;
		class?: string;
	} = $props();
	let target: HTMLElement;
	let instance: p5;

	async function setup() {
		const { default: p5 } = await import('p5');
		if (props.addons?.length) {
			(window as unknown as Window & { p5: typeof p5 }).p5 = p5;
			await Promise.all(props.addons.map((addon) => addon?.()));
		}
		instance = new p5((p) => props.sketch(p), target);
	}

	$effect(() => {
		setup().catch((err) => console.error(err));
		return () => instance?.remove?.();
	});
</script>

<div bind:this={target} style={props.style} class={props.class}></div>
