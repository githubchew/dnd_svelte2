<script>
	import { fade } from 'svelte/transition';
	import { flip } from 'svelte/animate';
  
	let flashcards = [
	  { id: 1, front: 'What is the capital of France?', back: 'Paris' },
	  { id: 2, front: 'What is 2 + 2?', back: '4' },
	  { id: 3, front: 'Who wrote "Romeo and Juliet"?', back: 'William Shakespeare' }
	];
  
	let newFront = '';
	let newBack = '';
	let nextId = 4;
  
	function addFlashcard() {
	  if (newFront && newBack) {
		flashcards = [
		  ...flashcards,
		  { id: nextId++, front: newFront, back: newBack, color: getRandomColor() }
		];
		newFront = '';
		newBack = '';
	  }
	}
  
	function shuffleFlashcards() {
	  flashcards = flashcards
		.map(value => ({ value, sort: Math.random() }))
			.sort((a, b) => a.sort - b.sort)
			.map(({ value }) => value);
	}
  
	function flipCard(id) {
	  flashcards = flashcards.map(card => 
		card.id === id ? { ...card, flipped: !card.flipped } : card
	  );
	}
  
	function getRandomColor() {
		const letters = '89ABCDEF'; // Use higher values for less saturation
		let color = '#';
		for (let i = 0; i < 6; i++) {
			color += letters[Math.floor(Math.random() * letters.length)];
		}
		return color;
	}
  
	// Initialize borderColor for each card
	flashcards = flashcards.map(card => ({
		...card,
		color: getRandomColor(),
		borderColor: 'transparent'
	}));
</script>
  
<main class="p-4">
	<h1 class="text-2xl font-bold mb-4">Flashcards App</h1>
  
	<div class="mb-4">
	  <input
		bind:value={newFront}
		placeholder="Front of card"
		class="border p-2 mr-2"
	  />
	  <input
		bind:value={newBack}
		placeholder="Back of card"
		class="border p-2 mr-2"
	  />
	  <button on:click={addFlashcard} class="bg-blue-500 text-white p-2 rounded">
		Add Flashcard
	  </button>
	</div>
  
	<button on:click={shuffleFlashcards} class="bg-green-500 text-white p-2 rounded mb-4">
	  Shuffle Flashcards
	</button>
  
	<div class="flex flex-wrap gap-4">
	  {#each flashcards as card (card.id)}
		<div
		  animate:flip={{ duration: 300 }}
		  transition:fade
		  class="w-48 h-64 rounded-lg shadow-md cursor-pointer p-4 card"
		  style="background-color: {card.color}; border: 2px solid {card.borderColor};"
		  on:click={() => flipCard(card.id)}
		  on:mouseenter={() => card.borderColor = getRandomColor()}
		  on:mouseleave={() => card.borderColor = 'transparent'}
		>
		  <div class="w-full h-full flex items-center justify-center text-center">
			{#if card.flipped}
			  <p>{card.back}</p>
			{:else}
			  <p>{card.front}</p>
			{/if}
		  </div>
		</div>
	  {/each}
	</div>
</main>
  
<style>
	:global(body) {
	  font-family: Arial, sans-serif;
	}
  
	.flex {
	  display: flex;
	  flex-wrap: wrap;
	}
  
	.flex > div {
	  flex: 0 0 calc(20% - 1rem);
	  margin: 0.5rem; /* Add margin to create spacing between cards */
	  transition: background-color 0.3s ease, border 0.3s ease, transform 0.1s ease; /* Add transition for transform */
	  border-radius: 12px; /* Rounded corners */
	  padding: 16px; /* Padding inside the card */
	}
  
	.flex > div:hover {
	  background-color: #e0e0e0; /* Change this to any color you prefer for hover */
	}

	.card:active {
	  transform: scale(0.95); /* Scale down on click */
	}
  
	@media (max-width: 1200px) {
	  .flex > div {
		flex: 0 0 calc(25% - 1rem);
	  }
	}
  
	@media (max-width: 992px) {
	  .flex > div {
		flex: 0 0 calc(33.33% - 1rem);
	  }
	}
  
	@media (max-width: 768px) {
	  .flex > div {
		flex: 0 0 calc(50% - 1rem);
	  }
	}
  
	@media (max-width: 576px) {
	  .flex > div {
		flex: 0 0 100%;
	  }
	}
</style>
