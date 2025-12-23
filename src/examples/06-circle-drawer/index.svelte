<script lang="ts">
	type Circle = {
		id: string
		cx: number
		cy: number
		r: number
	}
type Status = 'drawing' | 'editing'

let status = $state<Status>('drawing')
let circles = $state<Circle[]>([])

function drawCircle(event: MouseEvent) {
	const svgElement = event.target as SVGElement
	const {left, top} = svgElement.getBoundingClientRect()
	if (status !== 'drawing') return

	const rect = (event.target as SVGElement).getBoundingClientRect()
	const cx = event.clientX - rect.left
	const cy = event.clientY - rect.top
	const r = 20 + Math.random() * 30

	circles = [...circles, { id: crypto.randomUUID(), cx, cy, r }]
}

</script>

<h1>Circle Drawer Example</h1>
<div class="space-y">
	<div class="actions flex-center">
		<button>Undo</button>
		<button>Redo</button>
	</div>

	<!-- svelte-ignore a11y_click_events_have_key_events -->
	<!-- svelte-ignore a11y_no_static_element_interactions -->
	<svg onclick={drawCircle} viewBox="0 0 600 400">
		{#each circles as circle}
			<circle {...circle} stroke="#fff" stroke-width="2"></circle>
		{/each}
	</svg>

</div>

<style>
	svg {
		width: 600px;
		height: 400px;
		background-color: var(--surface-2);
		border: 1px solid var(--border);
		border-radius: 4px;
	}

	.adjust{
		width: 400px;
		position: absolute;
		top: 50%;
		left: 50%;
		translate: -50% -50%;
		padding: var(--size-4);
		text-align: center;
	}
</style>
