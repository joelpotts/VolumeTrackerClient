<script>
	import { onMount } from 'svelte';

	let canvas;
	let ctx;
	export let width = 1024;
	export let height = 255;
	export let volumeData = [];

	onMount(() => {
		ctx = canvas.getContext('2d');
		drawVolumeCanvas();
	});

	function drawVolumeCanvas() {
		requestAnimationFrame(drawVolumeCanvas);
		ctx.fillStyle = 'white';
		ctx.fillRect(0, 0, width, height);
		for (let i = 0; i < volumeData.length; i++) {
			ctx.fillStyle = `rgb(0, 0, ${100 + volumeData[i] / 2})`;
			ctx.fillRect(i, 255 - volumeData[i], 1, volumeData[i]);
		}
	}
</script>

<main>
	<canvas {width} {height} bind:this={canvas} />
</main>

<style>
	canvas {
		border: 1px solid black;
	}
</style>
