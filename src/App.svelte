<script>
	let cat
	let isLoading = false

	const fetchCat = async () => {
		isLoading = true
		const response = await fetch('https://api.thecatapi.com/v1/images/search?mime_types=png')

		if (response.status === 200) {
			const cats = await response.json()
			const img = new Image()
			cat = cats[0]
			img.src = cat.url
			img.onload = () => {
				isLoading = false
			}
		} else {
			cat = undefined
			isLoading = false
		}
	}

	fetchCat()
</script>

<main>
	<div class="wrapper">
		{#if isLoading}
			loading
		{:else if cat}
			<img src={cat.url}  alt={`Picture of cat ${cat.id}`}/>
		{/if}
	</div>
	<div class="actions">
		<button type="button" on:click={fetchCat} disabled={isLoading}>NO</button>
		<button type="button" on:click={fetchCat} disabled={isLoading}>YES</button>
	</div>
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		height: 100vh;
		width: 100vw;
	}

	.wrapper {
		flex: 1;
		overflow: hidden;
	}

	img {
		height: 80vh;
	}

	.actions {
		display: flex;
		height: 20vh;
	}

	button {
		flex: 1;
		margin: 0;
	}
</style>
