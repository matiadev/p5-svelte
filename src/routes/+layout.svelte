<script lang="ts">
	let { children } = $props();
</script>

<svelte:head>
	<title>P5 Svelte</title>
</svelte:head>

{@render children()}

<style>
	:global {
		body {
			height: 100svh;
			display: grid;
			place-content: center;
			font-family: sans-serif;
			color: #fff;
			background: #1e1e1e;
		}
	}
</style>
