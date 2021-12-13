<script>
    import { goto } from '$app/navigation';

	let serverName = '';
    let emitRate = 100;

	function handleSubmit() {
        goto(`/server/${serverName}?emitrate=${emitRate}`)
    }
</script>

<h1>Create a Server</h1>
<form on:submit|preventDefault={handleSubmit}>
	<label for="server-name-input">Server Name</label>
	<input type="text" name="server-name" id="server-name-input" bind:value={serverName} />

    <label for="emit-rate-input">Emit Rate</label>
    <input type="number" name="emit-rate" id="emit-rate-input" min="100" bind:value={emitRate}>

	<button disabled={!serverName || serverName.length == 0}>Create</button>
</form>

<style>
    form {
        display: flex;
        flex-direction: column;
        gap: .5em;
    }
</style>
