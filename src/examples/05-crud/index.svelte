<script lang="ts">
	type Person = {
		fName: string
		lName: string
	}

	let people = $state([
		{ fname: 'Benin', surname: 'Baglehood' },
		{ fname: 'Calvin', surname: 'Clamble' },
		{ fname: 'Denise', surname: 'Dingleman' },
	])
	let selected = $state<Person>()
	let person = $state<Person>({ fName: '', lName: '' })
</script>

<h1>CRUD Example</h1>
<div class="container surface-2">
	<div class="search">
		<label class="group">
			<span>Filter prefix:</span>
			<input type="text" />
		</label>
	</div>

	<label class="people">
		<span class="sr-only">Names:</span>
		<select bind:value={selected} size="5">
			{#each people as p}
				<option value={p}>{p.fname}, {p.surname}</option>
			{/each}
		</select>
	</label>

	<div class="details">
		<label class="group">
			<span>First Name:</span>
			<input type="text" bind:value={person.fName} />
		</label>
		<label class="group">
			<span>Last Name:</span>
			<input type="text" bind:value={person.lName} />
		</label>
	</div>

	<div class="actions space-x">
		<button
			onclick={() => {
				if (person.fName && person.lName) {
					people = [...people, { fname: person.fName, surname: person.lName }]
					person = { fName: '', lName: '' }
				}
			}}
		>
			Create
		</button>
		<button
			onclick={() => {
				if (selected) {
					people = people.map((p) =>
						p === selected ? { fname: person.fName, surname: person.lName } : p,
					)
					selected = undefined
					person = { fName: '', lName: '' }
				}
			}}
			disabled={!selected}
		>
			Update
		</button>
		<button
			onclick={() => {
				if (selected) {
					people = people.filter((p) => p !== selected)
					selected = undefined
					person = { fName: '', lName: '' }
				}
			}}
			disabled={!selected}
		>
			Delete
		</button>
	</div>
</div>

<style>
	.container {
		display: grid;
		grid-template-areas:
			'search search'
			'people details'
			'actions actions';
		grid-template-columns: 1fr 1fr;
		grid-template-rows: auto 1fr auto;
		gap: 1rem;
		padding: 1rem;
		border-radius: 0.5rem;
	}
</style>
