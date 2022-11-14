<script>
	// this data will be passed from App.svelte
	export let countryName 
	export let population
	export let region
	export let capital
	export let flagSrc
</script>

<div class="card">
	<img src={flagSrc} class="flag" alt={countryName}>
	<div class="textInfo">
		<p>{countryName}</p>
		<p>Population: <span>{population}</span></p>
		<p>Region: <span>{region}</span></p>
		<p>Capital: <span>{capital}</span></p>
	</div>
</div>

<style>
	.card {
		flex: 1 1 300px;
		max-width: 300px;
	}
	

	.flag {
		width: 100%;
		display: block;
		background-color: gray;
	}

	.textInfo {
		display: flex;
		flex-direction: column;
		gap: .5rem;
		padding: 2rem;
		background-color: var(--DarkModeElements);
		font-weight: 600;
		box-shadow: 0 0 1rem .1rem rgba(0, 0, 0, 0.1);
	}

	p:first-child {
		font-size: 1.25rem;
		font-weight: 800;
	}

	span {
		font-weight: 300;
	}

	@media screen and (min-width: 2500px) {
		.card {
			max-width: 300px;
		}
	}
</style>