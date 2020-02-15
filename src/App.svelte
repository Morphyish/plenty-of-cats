<script>
	import Cross from './Icons/Cross.svelte'
	import Like from './Icons/Like.svelte'

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
            <span>loading...</span>
        {:else if cat}
            <img src={cat.url} alt={`Picture of cat ${cat.id}`} />
        {/if}
    </div>
    <div class="actions">
        <button class="reject" type="button" on:click={fetchCat} disabled={isLoading}>
			<Cross />
        </button>
        <button class="like" type="button" on:click={fetchCat} disabled={isLoading}>
			<Like />
		</button>
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
        display: flex;
        align-items: center;
        justify-content: center;
        flex: 1;
        overflow: hidden;
        background: #333;
        color: #fff;
    }

    img {
        max-width: 100vw;
        max-height: 80vh;
        margin: auto;
    }

    .actions {
        display: flex;
        height: 20vh;
    }

    button {
        display: flex;
        flex: 1;
        align-items: center;
        justify-content: center;
        margin: 0;
    }

    button.reject {
        color: darkred;
    }

    button.like {
        color: green;
    }

    button[disabled] {
        color: #ccc;
    }
</style>
