<script>
	let serverName = '';

	function handleSubmit() {
		alert('submitted');
	}
</script>

<h1>Create a Server</h1>
<form on:submit|preventDefault={handleSubmit}>
	<label for="server-name-input">Server Name</label>
	<input type="text" name="server-name" id="server-name-input" bind:value={serverName} />
	<button>Create</button>
</form>
