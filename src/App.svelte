<script>
	let search = "";
	let isLoading = false;
	let error = false;
	let gifs = [];
	const API_URL =
		"https://api.giphy.com/v1/gifs/search?api_key=GW3gxd4WynXU6qBCE7bgkxquFh59lOr3&limit=25&offset=0&rating=g&lang=en&q=";
	async function formSubmitted(event) {
		event.preventDefault();
		isLoading = true;
		gifs = [];
		const url = `${API_URL}${search}`;
		const response = await fetch(url);
		const json = await response.json();
		console.log(json);

		if (json.meta.status !== 200) {
			error = true;
			console.log("Response with status code != 200");
			return;
		}

		gifs = json.data.map((gif) => gif.images.fixed_height.url);
		isLoading = false;
	}
</script>

<div class="box container is-fluid">
	<form on:submit={formSubmitted}>
		<label class="is-size-2" for="search">Search GIFS 🍔🍩☕</label>
		<div class="level is-mobile">
			<input
				class="input is-normal"
				bind:value={search}
				id="search"
				name="search"
				type="text"
				placeholder="Search Gifs..."
			/>
			<button class="button is-primary ml-2" type="submit">GO</button>
		</div>
	</form>
</div>
{#if isLoading}
	<img
		alt="is loading"
		src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fi.pinimg.com%2Foriginals%2F71%2F3a%2F32%2F713a3272124cc57ba9e9fb7f59e9ab3b.gif&f=1&nofb=1"
		width="100"
	/>
{/if}

{#if error}
<div class="container is-fluid">
	<img class="image is-128x128"
		alt="error occured"
		src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2F2.bp.blogspot.com%2F-CPO_z4zNSnc%2FWsY667p0JgI%2FAAAAAAAAYRs%2FubTMJD5ToyImbR-o4EiK18gBypYXd0RiwCLcBGAs%2Fs1600%2FMercenary%252BGarage%252BError%252BGIF.gif&f=1&nofb=1"
		width="300"
	/>
</div>
{/if}

<div class="results">
	{#each gifs as gif}
		<img alt="gif" src={gif} />
	{/each}
</div>

<style>
	.results {
		column-count: 3;
	}

	img {
		width: 100%;
		height: auto;
	}
</style>
