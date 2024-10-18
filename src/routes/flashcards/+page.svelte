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
		flashcards = [...flashcards, { id: nextId++, front: newFront, back: newBack }];
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
		  class="w-48 h-64 bg-yellow-100 rounded shadow-md cursor-pointer"
		  on:click={() => flipCard(card.id)}
		>
		  <div class="w-full h-full flex items-center justify-center p-4 text-center">
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
	  margin-bottom: 1rem;
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