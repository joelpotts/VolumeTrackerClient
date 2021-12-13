<script>
	import Header from '../components/Header.svelte';
	import Navbar from '../components/Navbar.svelte';

	import '../app.css';
</script>

<Header />
<Navbar />

<main>
	<slot />
</main>
