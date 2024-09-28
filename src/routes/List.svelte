<script>
	import { dndzone } from 'svelte-dnd-action';
	import { flip } from 'svelte/animate';
	const flipDurationMs = 200;

	export let items = [];
	export let type;

	function handleSort(e) {
		items = e.detail.items;
	}
</script>

<section
	class="listSection"
	use:dndzone={{ items, flipDurationMs, type }}
	on:consider={handleSort}
	on:finalize={handleSort}
>
	{#each items as item (item.id)}
		<div
			style={type === 'dark' ? 'background-color:rgba(0,0,0,0.7); color: white' : ''}
			animate:flip={{ duration: flipDurationMs }}
		>
			{item.title}
		</div>
	{/each}
</section>

<!-- ------------------------------------------ -->
<style>
	.listSection {
		display: flex;
		flex-wrap: wrap;
	}
	div {
		font-family: cursive, sans-serif;
		font-size: 2rem;
		color: var(--color, black);
		font-weight: var(--fontWeight, normal);
		height: 1.5em;
		width: 1em;
		text-align: center;
		border: 2px solid rgba(233, 207, 8, 0.7);
		box-shadow:
			rgba(0, 0, 0, 0.09) 0px 2px 1px,
			rgba(0, 0, 0, 0.09) 0px 4px 2px,
			rgba(0, 0, 0, 0.09) 0px 8px 4px,
			rgba(0, 0, 0, 0.09) 0px 16px 8px,
			rgba(0, 0, 0, 0.09) 0px 32px 16px;
		border-radius: 1.5rem;
		margin: 0.2em;
		padding: 0.3em;
	}

	div:nth-child(even) {
		color: var(--color2, rgba(7, 85, 25, 0.7));
	}

	div:nth-child(3n) {
		color: var(--color3, rgba(96, 14, 21, 0.7));
	}
	section {
		min-height: 12em;
	}
</style>
