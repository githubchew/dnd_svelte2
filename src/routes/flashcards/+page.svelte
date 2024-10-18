<script>
	import { onMount } from 'svelte';
  
	let flashcards = [];
	let currentIndex = 0;
	let showingQuestions = [];
	let newQuestion = '';
	let newAnswer = '';
  
	onMount(() => {
	  // Initialize showingQuestions array
	  showingQuestions = new Array(flashcards.length).fill(true);
	});
  
	async function addFlashcard() {
	  if (newQuestion && newAnswer) {
		flashcards = [...flashcards, { question: newQuestion, answer: newAnswer }];
		showingQuestions = [...showingQuestions, true];
		newQuestion = '';
		newAnswer = '';
	  }
	}
  
	function deleteFlashcard(index) {
	  flashcards = flashcards.filter((_, i) => i !== index);
	  showingQuestions = showingQuestions.filter((_, i) => i !== index);
	  if (currentIndex >= flashcards.length) {
		currentIndex = Math.max(0, flashcards.length - 1);
	  }
	}
  
	function flipCard(index) {
	  showingQuestions[index] = !showingQuestions[index];
	  showingQuestions = [...showingQuestions];
	}
  
	function nextCard() {
	  if (flashcards.length > 0) {
		currentIndex = (currentIndex + 1) % flashcards.length;
	  }
	}
  
	function prevCard() {
	  if (flashcards.length > 0) {
		currentIndex = (currentIndex - 1 + flashcards.length) % flashcards.length;
	  }
	}
  
	function shuffleDeck() {
	  if (flashcards.length > 0) {
		let shuffled = flashcards
		  .map(value => ({ value, sort: Math.random() }))
		  .sort((a, b) => a.sort - b.sort)
		  .map(({ value }) => value);
		flashcards = shuffled;
		showingQuestions = new Array(flashcards.length).fill(true);
		currentIndex = 0;
	  }
	}
  </script>
  
  <svelte:head>
	<title>SvelteKit Flashcards App</title>
  </svelte:head>
  
  <div class="flashcards-app">
	<h1>SvelteKit Flashcards App</h1>
	
	<form on:submit|preventDefault={addFlashcard} class="add-flashcard">
	  <h2>Add New Flashcard</h2>
	  <input type="text" bind:value={newQuestion} placeholder="Enter question" required />
	  <input type="text" bind:value={newAnswer} placeholder="Enter answer" required />
	  <button type="submit">Add Flashcard</button>
	</form>
  
	{#if flashcards.length > 0}
	  <div class="flashcards-row">
		{#each flashcards.slice(currentIndex, currentIndex + 5) as flashcard, index}
		  <div class="flashcard" on:click={() => flipCard(currentIndex + index)}>
			<div class="flashcard-content">
			  {#if showingQuestions[currentIndex + index]}
				<p>{flashcard.question}</p>
			  {:else}
				<p>{flashcard.answer}</p>
			  {/if}
			</div>
		  </div>
		{/each}
	  </div>
  
	  <div class="controls">
		<button on:click={prevCard}>Previous</button>
		<button on:click={nextCard}>Next</button>
		<button on:click={shuffleDeck}>Shuffle</button>
	  </div>
  
	  <div class="flashcard-list">
		<h2>All Flashcards</h2>
		{#each flashcards as flashcard, index}
		  <div class="flashcard-item">
			<p>Q: {flashcard.question}</p>
			<p>A: {flashcard.answer}</p>
			<button on:click={() => deleteFlashcard(index)}>Delete</button>
		  </div>
		{/each}
	  </div>
	{:else}
	  <p>No flashcards yet. Add some to get started!</p>
	{/if}
  </div>
  
  <style>
	.flashcards-app {
	  font-family: Arial, sans-serif;
	  max-width: 800px;
	  margin: 0 auto;
	  padding: 20px;
	}
  
	h1, h2 {
	  color: #333;
	}
  
	.add-flashcard {
	  margin-bottom: 20px;
	}
  
	input {
	  display: block;
	  width: 100%;
	  margin-bottom: 10px;
	  padding: 5px;
	}
  
	button {
	  background-color: #4CAF50;
	  border: none;
	  color: white;
	  padding: 10px 20px;
	  text-align: center;
	  text-decoration: none;
	  display: inline-block;
	  font-size: 16px;
	  margin: 4px 2px;
	  cursor: pointer;
	}
  
	.flashcards-row {
	  display: flex;
	  justify-content: center;
	  gap: 10px;
	  margin-bottom: 20px;
	}
  
	.flashcard {
	  width: 150px;
	  height: 100px;
	  perspective: 1000px;
	  cursor: pointer;
	}
  
	.flashcard-content {
	  width: 100%;
	  height: 100%;
	  background-color: #f1f1f1;
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  border-radius: 10px;
	  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
	  transition: transform 0.6s;
	  transform-style: preserve-3d;
	}
  
	.flashcard-content p {
	  font-size: 14px;
	  text-align: center;
	  padding: 10px;
	}
  
	.controls {
	  display: flex;
	  justify-content: center;
	  gap: 10px;
	  margin-bottom: 20px;
	}
  
	.flashcard-list {
	  margin-top: 40px;
	}
  
	.flashcard-item {
	  background-color: #f9f9f9;
	  border: 1px solid #ddd;
	  padding: 10px;
	  margin-bottom: 10px;
	  border-radius: 5px;
	}
  
	.flashcard-item p {
	  margin: 5px 0;
	}
  </style>
