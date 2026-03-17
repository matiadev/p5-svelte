<script lang="ts">
	let { children } = $props();
</script>

<svelte:head>
	<title>Svelte + p5.js</title>
</svelte:head>

{@render children()}

<style>
	:global {
		body {
			height: 100svh;
			display: grid;
			place-content: center;
			font-family: system-ui;
			color: #fff;
			background: #1b1e28;
			text-align: center;
		}
	}
</style>
