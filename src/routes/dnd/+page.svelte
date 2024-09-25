<!-- ------------------------------------------------------------------------- below dnd-->

<script>
	import { dndzone } from 'svelte-dnd-action';
	import List from '../List.svelte';
	let items1 = [
		{ id: 1, title: 'a' },
		{ id: 2, title: 'b' },
		{ id: 3, title: 'c' },
		{ id: 4, title: 'd' },
		{ id: 5, title: 'e' },
		{ id: 6, title: 'f' },
		{ id: 7, title: 'g' },
		{ id: 8, title: 'h' },
		{ id: 9, title: 'i' },
		{ id: 10, title: 'j' },
		{ id: 11, title: 'k' },
		{ id: 12, title: 'l' },
		{ id: 13, title: 'm' },
		{ id: 14, title: 'n' },
		{ id: 15, title: 'o' },
		{ id: 16, title: 'p' },
		{ id: 17, title: 'q' },
		{ id: 18, title: 'r' },
		{ id: 19, title: 's' },
		{ id: 20, title: 't' },
		{ id: 21, title: 'u' },
		{ id: 22, title: 'v' },
		{ id: 23, title: 'w' },
		{ id: 24, title: 'x' },
		{ id: 25, title: 'y' },
		{ id: 26, title: 'z' }
	];
	let items2 = [
		{ id: 4, title: '😊' },
		{ id: 5, title: '🤖' }
	];
	let items3 = [
		{ id: 1, title: 'a' },
		{ id: 2, title: 'b' },
		{ id: 3, title: 'c' },
		{ id: 4, title: 'd' }
	];
</script>

<svelte:head>
	<title>Dnd</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div class="text-column">
	<h6>drag and drop here</h6>
</div>
<hr />
<List items={items1} type="light" />
<hr />
<List items={items2} type="light" />
<hr />
<List items={items3} type="dark" />

<style>
	:global(body) {
		background-color: gray;
	}
</style>
