<script>
	import List from "./List.svelte"
</script>

<main>
	<h1>To-Do List</h1>
	<List/>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 75%;
		margin: 0 auto;
	}

	h1 {
		color: #f35a27;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	:global(body) {
		background: rgb(142,142,143);
		background: linear-gradient(90deg, rgb(172, 172, 172) 0%, rgba(238,238,237,1) 100%);
	}

</style>