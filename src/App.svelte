<script>
  import CountyCard from "./lib/CountyCard.svelte";
	import moonIcon from "./assets/moon-solid.svg"
	import sunIcon from "./assets/sun-solid.svg"

	let regions = ["Africa","Americas","Asia","Europe","Oceania"]
	let selectedRegion = "Europe" //selected europe by default
	let countrySearch

	let allData = []

	// FILTER BY REGION FUNCTION
	$:if(selectedRegion == regions[0]) {
		fetch("https://restcountries.com/v3.1/region/africa")
		.then(res => res.json())
		.then(d => {allData = d})
	}
	else if (selectedRegion == regions[1]) {
		fetch("https://restcountries.com/v3.1/region/america")
		.then(res => res.json())
		.then(d => {allData = d})
	}
	else if (selectedRegion == regions[2]) {
		fetch("https://restcountries.com/v3.1/region/asia")
		.then(res => res.json())
		.then(d => {allData = d})
	}
	else if (selectedRegion == regions[3]) {
		fetch("https://restcountries.com/v3.1/region/europe")
		.then(res => res.json())
		.then(d => {allData = d})
	}
	else if (selectedRegion == regions[4]) {
		fetch("https://restcountries.com/v3.1/region/oceania")
		.then(res => res.json())
		.then(d => {allData = d})
	}

	// SEARCH FUNCTION
	function searchCountry (ev) {
		console.log(countrySearch)
		if(ev.key === "Enter")
		{
			console.log("you press enter")
			fetch(`https://restcountries.com/v3.1/name/${countrySearch}`)
			.then(res => res.json())
			.then(resJSON => {allData = resJSON})
		}
	}

	let darkMode = true
	function toggletheme () {
		document.body.classList.toggle("light")
		darkMode = !darkMode
	}
</script>

<svelte:window on:keydown={searchCountry}/>

<nav>
	<h1>Where in the world?</h1>
	<button on:click={toggletheme}>
		{#if darkMode}
			<img class:darkMode src={sunIcon} alt=""> Light Mode
			{:else}
			<img class:darkMode src={moonIcon} alt=""> Dark Mode
		{/if}
	</button>
</nav>
<main>
	<div class="top">
		<input bind:value={countrySearch} type="text" placeholder="Search for a country...">
		<select bind:value={selectedRegion}>
			<option class="option" disabled selected hidden>Filter by Region</option>
			{#each regions as region}
				<option class="option" value={region}>{region}</option>
			{/each}
		</select>
	</div>
	<div class="bottom">
		{#each allData as data}
			<CountyCard countryName={data.name.common} population={data.population} region={data.region} capital={data.capital[0]} flagSrc={data.flags.png}/>
		{/each}
	</div>
</main>

<style>
	:global(body) {
		/* NEUTRAL COLORS */
		--DarkModeElements: hsl(209, 23%, 22%);
		--DarkModeBackground: hsl(207, 26%, 17%);

		--LightModeText: hsl(200, 15%, 8%);
		--LightModeInput: hsl(0, 0%, 52%);
		--LightModeBackground: hsl(0, 0%, 98%);

		--white: hsl(0, 0%, 100%);

		/* WELL FIGURE OUT THE COLORS FIRST */

		font-family: 'Nunito Sans', sans-serif;
		font-size: 16px;
		/* items: 14px */
		/* detail page 16px */
	}

	:global(body.light) {
		--DarkModeElements: hsl(0, 0%, 100%);
		--DarkModeBackground: hsl(0, 0%, 98%);

		--LightModeText: hsl(200, 15%, 8%);
		--LightModeInput: hsl(0, 0%, 52%);
		--LightModeBackground: hsl(0, 0%, 98%);

		--white: hsl(200, 15%, 8%);
	}


	nav {
		display: flex;
		justify-content: space-between;
		padding: 1rem 4rem;
		box-shadow: 0 0 .1rem .1rem rgba(0, 0, 0,.1);
		background-color: var(--DarkModeElements);
		position: relative;
		z-index: 1;
	}
	
	button {
		background: none;
		border: none;
		display: flex;
		align-items: center;
		gap: .5rem;
		color: inherit;
		cursor: pointer;
	}

	button img {
		width: 16px;
	}

	button img.darkMode {
		filter: brightness(0) invert(1);
	}

	main {
		padding: 3rem 4rem;
		background-color: var(--DarkModeBackground);
		min-height: 100vh;
	}

	input {
		padding: 1rem;
		border: none;
		box-shadow: 0 0 .1rem .1rem rgba(0, 0, 0,.1);
		background-color: var(--DarkModeElements);
		width: 500px;
		border-radius: .25rem;
		
	}

	input::placeholder {
		color: inherit;
		font-style: inherit;
	}

	select {
		padding: 1rem;
		border: none;
		box-shadow: 0 0 .1rem .1rem rgba(0, 0, 0,.1);
		background-color: var(--DarkModeElements);
		border-radius: .25rem;
		cursor: pointer;
	}

	.top {
		display: flex;
		justify-content: space-between;
	}

	.bottom {
		display: flex;
		flex-wrap: wrap;
		padding-top: 2rem;
		gap: 2rem;
		justify-content: space-between;
	}

	@media screen and (max-width: 1366px) {
		.top {
			flex-direction: column;
			gap: 1rem;
		}

		input {
			width: initial;
		}
	}

	@media screen and (max-width: 450px) {
		nav {
			padding: 1rem 2rem;
		}

		main {
			padding: 2rem;
		}

		h1 {
			font-size: 1rem;
		}

		.bottom {
			justify-content: center;
		}
	}
</style>