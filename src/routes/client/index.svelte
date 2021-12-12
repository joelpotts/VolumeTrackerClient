<script>
	import { onMount } from 'svelte';
	import io from 'socket.io-client';
	import VolumeCanvas from '../../components/VolumeCanvas.svelte';

	let socket;
	let volumeSources = {};

	onMount(() => {
		socket = io('http://localhost:5000', { query: { socketType: 'client' } });

		socket.on('volumedata', (data) => {
			let serverName = data.serverid;
			let serverData = new Uint8Array(data.volumeData);
			volumeSources[serverName] = serverData;
		});

		socket.on('disconnectserver', (serverName) => {
			console.log(serverName);
			delete volumeSources[serverName];
		});
	});
</script>

{#each Object.entries(volumeSources) as [name, data], index (name)}
	<h2>{name}</h2>
	<VolumeCanvas volumeData={data} />
{/each}
