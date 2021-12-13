<script>
    import { goto } from '$app/navigation';

	let serverName = '';

	function handleSubmit() {
        goto(`/server/${serverName}`)
    }
</script>

<h1>Create a Server</h1>
<form on:submit|preventDefault={handleSubmit}>
	<label for="server-name-input">Server Name</label>
	<input type="text" name="server-name" id="server-name-input" bind:value={serverName} />
	<button disabled={!serverName || serverName.length == 0}>Create</button>
</form>

<style>
    form {
        display: flex;
        flex-direction: column;
        gap: .5em;
    }
</style>
