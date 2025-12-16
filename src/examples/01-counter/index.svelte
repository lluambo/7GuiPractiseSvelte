<script lang="ts">
	let count = $state(0)
</script>

<h1>Welcome</h1>
<div class="flex-gap">
	<input type="number" bind:value={count} readonly />
	<button onclick={() => count++}>Count</button>
</div>
