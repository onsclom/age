<script>
	import { fly } from 'svelte/transition';
	let name = 'world'
	const averageAgeSpan = 80
	let dob
	$: validDate = isValidDob(dob)
	let age = 0
	$: tooOld = (age, averageAgeSpan) => {return age>averageAgeSpan}
	let submitted = false
	function submit() {
		submitted = true
    dob = new Date(dob)
    console.log(dob)   
		setInterval(updateAge, 16);
	}
	
	function updateAge() {
		age = ((new Date( new Date()-dob )/(1000*60*60*24))/365).toFixed(9)
	}
	
	function isValidDob(dob) {
		let now = new Date()
		let birth = new Date(dob)
		if (dob != null && now.getTime()>birth.getTime())
			return true
		return false
	}

</script>

{#if !submitted}
	<main>
	
	<form on:submit|preventDefault={submit} 
		transition:fly={{ y: -200, duration: 1000 }}>
		<p>
			Enter your birthday
		</p>
		<input type="date" bind:value={dob}>
		<button type="submit" disabled={!validDate}>
			Submit
		</button>
	</form>
	</main>
{:else}
	<main>
	<div transition:fly={{ y:200, duration: 1000 }}>
		<p>
			You are <b>{age}</b> years old.
		</p>
		{#if age<averageAgeSpan}
		<h2>👶 <progress value={age/averageAgeSpan}></progress> 💀</h2>
		<p>
			You have lived roughly <b>{(age/averageAgeSpan*100).toFixed(9)}%</b> of your life.
		</p>
		{:else}
		<p>
			damn you are old lol
		</p>
		{/if}
	</div>
	</main>
	{/if}

	

<style>
	main {
		text-align: center;
		font-family: monospace;
		height: 100%;
		width: 100%;
		display: flex;
		position: absolute;
		justify-content: center;
		align-items: center;
	}
	
	form {
		display: block;
	}
	
	div {
		margin: auto;
		padding: 1.5rem;
	}
</style>