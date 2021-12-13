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

<h2>Watch Volume Levels Live</h2>
<p>Here you can see the volume levels for each server that is connected.</p>

{#if volumeSources && Object.keys(volumeSources).length > 0}
    {#each Object.entries(volumeSources) as [name, data], index (name)}
        <h3>{name}</h3>
        <VolumeCanvas volumeData={data} />
    {/each}
{:else}
    <p>There are no servers connected right now.</p>
{/if}

