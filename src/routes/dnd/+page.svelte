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
		{ id: 12, title: 'L' },
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
		{ id: 26, title: 'z' },
		{ id: 27, title: '🍎' },
		{ id: 28, title: '🍌' },
		{ id: 29, title: '🐱' },
		{ id: 30, title: '🐶' },
		{ id: 31, title: '🥚' },
		{ id: 32, title: '🐘' },
		{ id: 33, title: '🌺' },
		{ id: 34, title: '👧' },
		{ id: 35, title: '👒' },
		{ id: 36, title: '🍦' },
		{ id: 37, title: '🪼' }
	];
	let items2 = [
		{ id: 200, title: '😊' },
		{ id: 201, title: '🤖' },
		{ id: 202, title: '🧸' },
		{ id: 203, title: '🦘' }
	];
	let items3 = [
		{ id: 100, title: 'a' },
		{ id: 101, title: 'b' },
		{ id: 102, title: 'c' },
		{ id: 103, title: 'd' },
		{ id: 104, title: 'e' },
		{ id: 105, title: 'f' },
		{ id: 106, title: 'g' },
		{ id: 107, title: 'h' },
		{ id: 108, title: 'i' },
		{ id: 109, title: 'j' },
		{ id: 110, title: 'k' },
		{ id: 111, title: 'L' },
		{ id: 112, title: 'm' },
		{ id: 113, title: 'n' },
		{ id: 114, title: 'o' },
		{ id: 115, title: 'p' },
		{ id: 116, title: 'q' },
		{ id: 117, title: 'r' },
		{ id: 118, title: 's' },
		{ id: 119, title: 't' },
		{ id: 120, title: 'u' },
		{ id: 121, title: 'v' },
		{ id: 122, title: 'w' },
		{ id: 123, title: 'x' },
		{ id: 124, title: 'y' },
		{ id: 125, title: 'z' },
		{ id: 27, title: '🍎' },
		{ id: 28, title: '🍌' },
		{ id: 29, title: '🐱' },
		{ id: 30, title: '🐶' },
		{ id: 31, title: '🥚' },
		{ id: 32, title: '🐘' },
		{ id: 33, title: '🌺' },
		{ id: 34, title: '👧' },
		{ id: 35, title: '👒' },
		{ id: 36, title: '🍦' },
		{ id: 37, title: '🪼' }
	];
</script>

<svelte:head>
	<title>Dnd</title>
	<meta name="description" content="About this app" />
</svelte:head>
<div class="bodyDND">
	<div class="text-column">
		<h6>drag and drop here</h6>
	</div>
	<hr />
	<div class="ListCustom">
		<List items={items1} type="light" --color="#474747" --color2="#474747" --color3="#474747" />
		<hr />
		<List
			items={items2}
			type="light"
			--color="blue"
			--color2="darkorange"
			--color3="green"
			--fontWeight="bold"
		/>
		<br />

		<List items={items3} type="light" --color="#474747" --color2="#474747" --color3="gray" />
	</div>
</div>

<style>
	.bodyDND {
		margin: 0;
		border-radius: 20% 11% 30% 17% / 29% 11% 31% 23%;
		box-shadow:
			rgba(0, 0, 0, 0.07) 0px 1px 1px,
			rgba(0, 0, 0, 0.07) 0px 2px 2px,
			rgba(0, 0, 0, 0.07) 0px 4px 4px,
			rgba(0, 0, 0, 0.07) 0px 8px 8px,
			rgba(0, 0, 0, 0.07) 0px 16px 16px;
		padding: 3rem;
		background: rgb(255, 254, 177);
		background: linear-gradient(
			99deg,
			rgba(255, 254, 177, 1) 0%,
			rgba(228, 220, 252, 1) 26%,
			rgb(139, 193, 247) 52%,
			rgb(224, 255, 199) 76%,
			rgba(246, 189, 202, 1) 100%
		);
	}
</style>
