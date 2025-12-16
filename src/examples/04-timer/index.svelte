<script lang="ts">
	let elapsed = $state(0)
	let duration = $state(5)
	let intercal: ReturnType<typeof setInterval>

	function startTimer() {
		intercal = setInterval(() => {
			if (elapsed < duration) {
				elapsed++
			} else {
				clearInterval(intercal)
			}
		}, 1000)
	}

	function resetTimer() {
		clearInterval(intercal)
		elapsed = 0
		startTimer()
	}
	$effect(() => {
		if (!duration) return
		startTimer()
		return () => clearInterval(intercal)
	})
</script>

<div class="grid-gap">
	<div>
		<label>
			<span>Elapsed time:</span>
			<progress max={duration} value={elapsed}></progress>
		</label>

		<div>{elapsed}s</div>
	</div>

	<label>
		<span>Set duration (seconds):</span>
		<input type="range" bind:value={duration} min="1" max="20" />
	</label>

	<button onclick={resetTimer}>Reset</button>
</div>
