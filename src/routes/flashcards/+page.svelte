<script>
	// Load flashcards from local storage or initialize with default values
	let flashcards = JSON.parse(localStorage.getItem('flashcards')) || [
		{ front: '❓', back: 'fart', flipped: false, color: getRandomColor() },
		{ front: '?', back: 'going', flipped: false, color: getRandomColor() },
		{ front: 'pancake ?', back: '🥞', flipped: false, color: getRandomColor() }
	];

	let newFrontContent = '';
	let newBackContent = '';

	function toggleFlip(index) {
		flashcards[index].flipped = !flashcards[index].flipped;
		saveFlashcards();
	}

	function getRandomColor() {
		const letters = '89ABCDEF'; // Use only brighter hex digits
		let color = '#';
		for (let i = 0; i < 6; i++) {
			color += letters[Math.floor(Math.random() * letters.length)];
		}
		return color;
	}

	function addFlashcard() {
		flashcards = [
			...flashcards,
			{ front: newFrontContent, back: newBackContent, flipped: false, color: getRandomColor() }
		];
		newFrontContent = '';
		newBackContent = '';
		saveFlashcards();
	}

	function shuffleFlashcards() {
		flashcards = flashcards
			.map((value) => ({ value, sort: Math.random() }))
			.sort((a, b) => a.sort - b.sort)
			.map(({ value }) => value);
		saveFlashcards();
	}

	function handleKeyDown(event) {
		if (event.key === 'Enter') {
			addFlashcard();
		}
	}

	function saveFlashcards() {
		localStorage.setItem('flashcards', JSON.stringify(flashcards));
	}
</script>

<div>
	<input
		type="text"
		placeholder="Front content"
		bind:value={newFrontContent}
		on:keydown={handleKeyDown}
	/>
	<input
		type="text"
		placeholder="Back content"
		bind:value={newBackContent}
		on:keydown={handleKeyDown}
	/>
	<button on:click={addFlashcard}>Add Flashcard</button>
	<button on:click={shuffleFlashcards}>Shuffle Flashcards</button>
</div>

<div class="flashcard-container">
	{#each flashcards as { front, back, flipped, color }, index}
		<div
			class="flashcard {flipped ? 'flipped' : ''}"
			on:click={() => toggleFlip(index)}
			style="--flashcard-color: {color}"
		>
			<div class="flashcard-content front">
				{front}
			</div>
			<div class="flashcard-content back">
				{back}
			</div>
		</div>
	{/each}
</div>

<style>
	.flashcard-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		max-width: 1000px; /* 5 flashcards * 200px each */
		margin: 0 auto;
	}

	.flashcard {
		width: 200px;
		height: 150px;
		border: 1px solid #ccc;
		border-radius: 8px;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		transition:
			transform 0.6s,
			background-color 0.3s; /* Add transition for background-color */
		transform-style: preserve-3d;
		position: relative;
		margin: 10px; /* Add some margin for spacing */
		background-color: var(--flashcard-color);
	}

	.flashcard.flipped {
		transform: rotateY(180deg);
	}

	.flashcard:hover {
		background-color: #e0e0e0; /* Change to a lighter color on hover */
	}

	.flashcard-content {
		position: absolute;
		backface-visibility: hidden;
		font-size: 2.5em; /* Increase font size by 2.5 times */
	}

	.front {
		visibility: visible;
	}

	.back {
		transform: rotateY(180deg);
		visibility: hidden; /* Make the back content invisible by default */
	}

	.flashcard.flipped .front {
		visibility: hidden;
	}

	.flashcard.flipped .back {
		visibility: visible;
	}
</style>
