<script lang="ts">
	import { get } from 'svelte/store'

	type Options = 'one-way' | 'round-trip'

	function getDate() {
		const today = new Date()
		const d = today
			.toLocaleDateString('en-US', { year: 'numeric', month: '2-digit', day: '2-digit' })
			.split('/') // Format: YYYY-MM-DD
		return `${d[2]}-${d[0].padStart(2, '0')}-${d[1].padStart(2, '0')}`
	}

	function handleSubmit(e: Event) {
		e.preventDefault()
		alert(
			`You have booked a ${selectOptions} flight${selectOptions === 'round-trip' ? ` returning on ${returnDate}` : ''} departing on ${startDate}`,
		)
	}

	let selectOptions = $state<Options>('one-way')
	let startDate = $state(getDate())
	let returnDate = $state(getDate())

	$inspect({ selectOptions, startDate, returnDate })
</script>

<form onsubmit={handleSubmit} class="grid-gap">
	<select bind:value={selectOptions}>
		<option value="one-way">One-way</option>
		<option value="round-trip">Round-trip</option>
	</select>

	<label>
		<span class="sr-only">Select the start date</span>
		<input type="date" bind:value={startDate} required />
	</label>
	<label>
		<span class="sr-only">Select the return date</span>
		<input
			type="date"
			bind:value={returnDate}
			required={selectOptions === 'round-trip'}
			disabled={selectOptions === 'one-way'}
		/>
	</label>
	<button
		type="submit"
		disabled={!startDate || (selectOptions === 'round-trip' && returnDate < startDate)}
		>Book Flight</button
	>
</form>
