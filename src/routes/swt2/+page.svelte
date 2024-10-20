<script>
	import { onMount } from 'svelte';

	let inputText = '';
	let customWords = '';
	let words = [
		'example',
		'sight',
		'words',
		'list',
		'here',
		'and',
		'away',
		'big',
		'blue',
		'can',
		'come',
		'down',
		'find',
		'for',
		'funny',
		'go',
		'help',
		'here',
		'I',
		'in',
		'is',
		'it',
		'jump',
		'little',
		'look',
		'make',
		'me',
		'my',
		'not',
		'one',
		'play',
		'red',
		'run',
		'said',
		'see',
		'the',
		'three',
		'to',
		'two',
		'up',
		'we',
		'where',
		'yellow',
		'you',
		'kindergarten',
		'all',
		'am',
		'are',
		'at',
		'ate',
		'be',
		'black',
		'brown',
		'but',
		'came',
		'did',
		'do',
		'eat',
		'four',
		'get',
		'good',
		'have',
		'he',
		'into',
		'like',
		'must',
		'new',
		'no',
		'now',
		'on',
		'our',
		'out',
		'please',
		'pretty',
		'ran',
		'ride',
		'saw',
		'say',
		'she',
		'so',
		'soon',
		'that',
		'there',
		'they',
		'this',
		'too',
		'under',
		'want',
		'was',
		'well',
		'went',
		'what',
		'white',
		'who',
		'will',
		'with',
		'yes',
		
		'after',
		'again',
		'an',
		'any',
		'as',
		'ask',
		'by',
		'could',
		'every',
		'fly',
		'from',
		'give',
		'going',
		'had',
		'has',
		'her',
		'him',
		'his',
		'how',
		'just',
		'know',
		'let',
		'live',
		'may',
		'of',
		'old',
		'once',
		'open',
		'over',
		'put',
		'round',
		'some',
		'stop',
		'take',
		'thank',
		'them',
		'then',
		'think',
		'walk',
		'were',
		'when',

		'always',
		'around',
		'because',
		'been',
		'before',
		'best',
		'both',
		'buy',
		'call',
		'cold',
		'does',
		"don't",
		'fast',
		'first',
		'five',
		'found',
		'gave',
		'goes',
		'green',
		'its',
		'made',
		'many',
		'off',
		'or',
		'pull',
		'read',
		'right',
		'sing',
		'sit',
		'sleep',
		'tell',
		'their',
		'these',
		'those',
		'upon',
		'us',
		'use',
		'very',
		'wash',
		'which',
		'why',
		'wish',
		'work',
		'would',
		'write',
		'your',
		'third grade',
		"let's",
		'about',
		'better',
		'bring',
		'carry',
		'clean',
		'cut',
		'done',
		'draw',
		'drink',
		'eight',
		'fall',
		'far',
		'full',
		'got',
		'grow',
		'hold',
		'hot',
		'hurt',
		'if',
		'keep',
		'kind',
		'laugh',
		'light',
		'long',
		'much',
		'myself',
		'never',
		'only',
		'own',
		'pick',
		'seven',
		'shall',
		'show',
		'six',
		'small',
		'start',
		'ten',
		'today',
		'together',
		'try',
		'warm',
		'NOUNS',
		'apple',
		'baby',
		'back',
		'ball',
		'bear',
		'bed',
		'bell',
		'bird',
		'birthday',
		'boat',
		'box',
		'boy',
		'bread',
		'brother',
		'cake',
		'car',
		'cat',
		'chair',
		'chicken',
		'children',
		'Christmas',
		'coat',
		'corn',
		'cow',
		'day',
		'dog',
		'doll',
		'door',
		'duck',
		'egg',
		'eye',
		'farm',
		'farmer',
		'father',
		'feet',
		'fire',
		'fish',
		'floor',
		'flower',
		'game',
		'garden',
		'girl',
		'goodbye',
		'grass',
		'ground',
		'hand',
		'head',
		'hill',
		'home',
		'horse',
		'house',
		'kitty',
		'leg',
		'letter',
		'man',
		'men',
		'milk',
		'money',
		'morning',
		'mother',
		'name',
		'nest',
		'night',
		'paper',
		'party',
		'picture',
		'pig',
		'rabbit',
		'rain',
		'ring',
		'robin',
		'Santa Claus',
		'school',
		'seed',
		'sheep',
		'shoe',
		'sister',
		'snow',
		'song',
		'squirrel',
		'stick',
		'street',
		'sun',
		'table',
		'thing',
		'time',
		'top',
		'toy',
		'tree',
		'watch',
		'water',
		'way',
		'wind',
		'window',
		'wood'
	];
	let targetText = getRandomWord();
	let displayedText = targetText.slice(0, Math.ceil(targetText.length / 2));
	let zombiePosition;
	let canvas;
	let ctx;
	let isZombieGone = false;
	let particles = [];
	let flashScreen = false;
	let score = 0;
	let health = 3;
	let displayInterval;
	let monsterEmojis = ['👹', '👺', '👻', '👽', '🤖', '👾', '🧟', '🧛', '🧜', '🧚'];
	let currentMonster = getRandomMonster();
	let hintColor = getRandomColor();
	let correctlyTypedWords = [];
	let gameTimer;
	let timeLeft = 180;
	let newWordIndex = -1; // Track the index of the newly added word

	onMount(() => {
		if (canvas) {
			ctx = canvas.getContext('2d');
			zombiePosition = canvas.width;
			animate();
			startDisplayCycle();
		}
	});

	function getRandomWord() {
		const wordList = customWords ? customWords.split(',').map((word) => word.trim()) : words;
		return wordList[Math.floor(Math.random() * wordList.length)];
	}

	function getRandomMonster() {
		return monsterEmojis[Math.floor(Math.random() * monsterEmojis.length)];
	}

	function drawZombie() {
		if (ctx && !isZombieGone) {
			ctx.font = '96px serif';
			ctx.fillText(currentMonster, zombiePosition, 90);
		}
	}

	function createParticles() {
		for (let i = 0; i < 100; i++) {
			particles.push({
				x: zombiePosition,
				y: 50,
				dx: (Math.random() - 0.5) * 2,
				dy: (Math.random() - 0.5) * 2,
				life: 100
			});
		}
	}

	function drawParticles() {
		particles = particles.filter((particle) => {
			ctx.fillStyle = 'red';
			ctx.fillRect(particle.x, particle.y, 2, 2);
			particle.x += particle.dx;
			particle.y += particle.dy;
			particle.life -= 1;
			return particle.life > 0;
		});
	}

	function animate() {
		if (ctx) {
			ctx.clearRect(0, 0, canvas.width, canvas.height);
			if (!isZombieGone) {
				zombiePosition -= 0.33;
				if (zombiePosition < -50) {
					flashScreen = true;
					setTimeout(() => (flashScreen = false), 200);
					zombiePosition = canvas.width;
				}
			}
			drawZombie();
			drawParticles();
			requestAnimationFrame(animate);
		}
	}

	function startDisplayCycle() {
		clearInterval(displayInterval);
		displayInterval = setInterval(updateDisplayedText, 9000);
		updateDisplayedText();
	}

	function updateDisplayedText() {
		hintColor = getRandomColor();
		const halfIndex = Math.ceil(targetText.length / 2);
		displayedText = targetText.slice(0, halfIndex) + '_ '.repeat(targetText.length - halfIndex);
		setTimeout(() => {
			displayedText = '_ '.repeat(halfIndex) + targetText.slice(halfIndex);
			setTimeout(() => {
				displayedText = targetText;
				setTimeout(() => {
					displayedText = '';
				}, 1000);
			}, 4000);
		}, 4000);
	}

	function handleInput(event) {
		inputText = event.target.value;
		if (inputText === targetText) {
			clearInterval(displayInterval);
			isZombieGone = true;
			createParticles();
			score++;
			if (score % 5 === 0) {
				health++;
			}

			correctlyTypedWords = [...correctlyTypedWords, targetText];
			newWordIndex = correctlyTypedWords.length - 1; // Set the index of the new word

			// Reset the newWordIndex after 3 seconds
			setTimeout(() => {
				newWordIndex = -1;
			}, 3000);

			isZombieGone = false;
			zombiePosition = canvas.width;
			inputText = '';
			currentMonster = getRandomMonster();

			// Add a 2-second delay before starting the new word cycle
			setTimeout(() => {
				targetText = getRandomWord();
				startDisplayCycle();
			}, 2000);
		}
	}

	function getRandomColor() {
		const letters = '0123456789ABCDEF';
		let color = '#';
		for (let i = 0; i < 6; i++) {
			color += letters[Math.floor(Math.random() * 16)];
		}
		return color;
	}

	function handleCustomWordsInput(event) {
		customWords = event.target.value;
		targetText = getRandomWord();
		startDisplayCycle();
	}

	function startNewGame() {
		clearInterval(gameTimer);
		resetGameState();
		startDisplayCycle();
		gameTimer = setInterval(() => {
			timeLeft--;
			if (timeLeft <= 0) {
				clearInterval(gameTimer);
				alert('Time is up! Game over.');
				resetGameState();
			}
		}, 1000);
	}

	function resetGameState() {
		clearInterval(displayInterval);
		clearInterval(gameTimer);
		timeLeft = 180;
		score = 0;
		health = 3;
		isZombieGone = false;
		zombiePosition = canvas.width;
		targetText = getRandomWord();
		displayedText = targetText.slice(0, Math.ceil(targetText.length / 2));
		particles = [];
		correctlyTypedWords = [];
		hintColor = getRandomColor();
		currentMonster = getRandomMonster();
	}
</script>

<div class="container {flashScreen ? 'flash' : ''}">
	<div class="correct-words">
		{#each correctlyTypedWords as word, index}
			<span class:highlight={index === newWordIndex}
				>{word}{index < correctlyTypedWords.length - 1 ? ', ' : ''}</span
			>
		{/each}
	</div>
	<div class="target-word" style="color: {hintColor};">
		<span class="hint-label">Type:</span>
		{displayedText}
	</div>
	<input type="text" bind:value={inputText} on:input={handleInput} placeholder="Type the word" />
	<input
		type="text"
		class="small-input"
		bind:value={customWords}
		on:input={handleCustomWordsInput}
		placeholder=" custom words by commas"
	/>
	<canvas bind:this={canvas} width="400" height="100"></canvas>
	<div class="score">Score: {score}</div>
	<div class="typed-words">Typed: {inputText}</div>
	<div class="health">Health: {'❤️'.repeat(health)}</div>
	<div class="timer">
		Time Left: {Math.floor(timeLeft / 60)}:{String(timeLeft % 60).padStart(2, '0')}
	</div>
	<button on:click={startNewGame}>New Game</button>
</div>

<style>
	canvas {
		border: 2px solid rgb(3, 157, 103);
		display: block;
		margin: 0 auto;
	}

	.container {
		text-align: center;
		font-size: 2em;
		transition: background-color 0.2s;
	}

	.flash {
		background-color: rgb(245, 71, 71);
	}

	input {
		font-size: 3em;
		margin-bottom: 10px;
		border-radius: 2rem;
		border: none;
		padding-left: 1rem;
		padding-right: 1rem;
	}

	.small-input {
		font-size: 1em;
		margin-top: 10px;
		margin-bottom: 20px;
		border-radius: 1rem;
	}

	.target-word {
		margin-bottom: 20px;
		font-size: 4em;
		font-weight: bold;
	}

	.hint-label {
		font-size: 0.5em;
		opacity: 0.04;
	}

	.score {
		padding-top: 2rem;
	}

	.score,
	.health {
		margin: 10px 0;
	}

	.correct-words {
		font-size: 1em;
		color: #333;
	}

	.timer {
		margin-top: 20px;
		font-size: 1em;
		color: #333;
	}

	input::placeholder {
		opacity: 0.04;
	}

	.highlight {
		color: red; /* Change to your desired highlight color */
		font-size: 1.4em; /* Increase font size */
		transition:
			color 0.3s ease-in-out,
			font-size 0.3s ease-in-out;
	}
</style>
