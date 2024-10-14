<script>
	import { onMount } from 'svelte';

	let todos = [];
	let newTodo = 'run'; // Set the default value to 'run'
	const feelGoodEmojis = ['😊', '🌞', '🎉', '🌈', '😍', '🤩'];
	$: console.log(todos);
	let storedTodos;

	onMount(() => {
		if (typeof window !== 'undefined') { // Ensure window is available
			storedTodos = window.localStorage.getItem('todos');
			if (storedTodos) {
				todos = JSON.parse(storedTodos).map((todo) => ({
					...todo,
					date: new Date(todo.timestamp)
				}));
			}
		}
	});

	function addTodo() {
		if (newTodo) {
			const date = new Date();
			const timestamp = `${date.getDate()} ${date.toLocaleString('default', { month: 'long' })} ${date.getFullYear()}`;
			todos = [...todos, { text: newTodo, emoji: getRandomEmoji(), timestamp, date }];
			newTodo = 'run'; // Reset the newTodo value after adding a todo
			if (typeof window !== 'undefined') { // Ensure window is available
				window.localStorage.setItem(
					'todos',
					JSON.stringify(
						todos.map((todo) => ({
							text: todo.text,
							emoji: todo.emoji,
							timestamp: todo.timestamp
						}))
					)
				);
			}
		}
	}

	function getRandomEmoji() {
		return feelGoodEmojis[Math.floor(Math.random() * feelGoodEmojis.length)];
	}

	function removeTodo(index) {
		todos = todos.filter((todo, i) => i !== index);
		if (typeof window !== 'undefined') { // Ensure window is available
			window.localStorage.setItem(
				'todos',
				JSON.stringify(
					todos.map((todo) => ({
						text: todo.text,
						emoji: todo.emoji,
						timestamp: todo.timestamp
					}))
				)
			);
		}
	}

	$: lastWeekTodos = todos.filter((todo) => {
		const oneWeekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000);
		return todo.date >= oneWeekAgo;
	}).length;

	$: lastMonthTodos = todos.filter((todo) => {
		const oneMonthAgo = new Date();
		oneMonthAgo.setMonth(oneMonthAgo.getMonth() - 1);
		return todo.date >= oneMonthAgo;
	}).length;
</script>

<h1>1 min Run</h1>
<input type="text" bind:value={newTodo} placeholder="Enter new todo" />
<button on:click={addTodo}>Add Todo</button>

<div>Total: {todos.length}</div>
<div>Last week: {lastWeekTodos}</div>
<div>Last month: {lastMonthTodos}</div>

<ul>
	{#each todos as todo, i}
		<li>
			{todo.text}
			{todo.emoji} - {todo.timestamp}
			<button on:click={() => removeTodo(i)}>Remove</button>
		</li>
	{/each}
</ul>

<style>
	/* Styles for the component */
</style>
