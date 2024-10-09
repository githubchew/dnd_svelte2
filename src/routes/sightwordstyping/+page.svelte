<script>
	import { onMount } from 'svelte';

	let words = []; // This will hold the top 1000 sight words
	let currentWord = '';
	let round = 1;
	let userInput = '';
	let message = '';
	let trophies = 0; // Count of trophies
	let positiveEmojis = ''; // String to hold positive emojis
	let timer; // Timer variable
	let timeLeft = 120; // Default to 2 minutes in seconds
	let gameStarted = false; // Track if the game has started
	let timerStarted = false; // Track if the timer has started

	// Function to shuffle the words array
	function shuffle(array) {
		for (let i = array.length - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1));
			[array[i], array[j]] = [array[j], array[i]];
		}
	}

	// Load the words
	onMount(() => {
		words = [
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
			'first grade',
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
			'second grade',
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
		]; // Replace with actual words
	});

	function startNewGame() {
		shuffle(words); // Shuffle words at the start of each game
		nextWord();
		gameStarted = true;
		timerStarted = false; // Reset timer started flag
	}

	function resetTimer() {
		clearInterval(timer);
		timer = setInterval(() => {
			timeLeft--;
			if (timeLeft <= 0) {
				clearInterval(timer);
				message = 'Time is up! Game over.';
				setTimeout(() => {
					stopGame();
				}, 7000); // 7-second delay before stopping the game
			}
		}, 1000);
	}

	function stopGame() {
		gameStarted = false;
		trophies = 0; // Reset trophies
		positiveEmojis = ''; // Clear positive emojis
	}

	function nextWord() {
		if (words.length === 0) {
			message = 'Congratulations! You have completed all words.';
			clearInterval(timer);
			return;
		}
		currentWord = words.shift();
		userInput = '';
		message = '';
		pronounceWord(currentWord); // Pronounce the word
	}

	function getMaskedWord(word, round) {
		if (round === 1) return word;
		const length = word.length;
		let maskCount;

		if (length >= 5) {
			maskCount = Math.ceil(length / 2); // Mask half or more of the letters
		} else {
			maskCount = round === 2 ? Math.floor(length / 4) : Math.floor(length / 2);
		}

		let maskedWord = word.split(''); // Ensure we don't mask the same letter twice
		for (let i = 0; i < maskCount; i++) {
			let index;
			do {
				index = Math.floor(Math.random() * length);
			} while (maskedWord[index] === '_'); // Ensure we don't mask the same letter twice
			maskedWord[index] = '_';
		}
		return maskedWord.join('');
	}

	function checkInput() {
		if (!timerStarted && userInput.length > 0) {
			resetTimer();
			timerStarted = true;
		}
		if (userInput === currentWord) {
			trophies++; // Increment trophy count
			positiveEmojis += '🏆'; // Add a trophy emoji to the string
			message = 'Correct! 🏆'; // Add trophy emoji
			round++;
			if (round > 3) {
				round = 1;
				nextWord();
			} else {
				userInput = ''; // Clear input after the first round
			}
		} else {
			message = 'Keep trying!';
		}
	}

	function setRoundDurationAndStart(seconds) {
		timeLeft = seconds;
		startNewGame();
	}

	function restartGame() {
		stopGame();
		round = 1; // Reset to beginner stage
		message = 'Game reset. Choose a duration and start again.';
	}

	function pronounceWord(word) {
		const voices = window.speechSynthesis.getVoices();
		const utterance1 = new SpeechSynthesisUtterance(word);
		utterance1.voice = voices.find((voice) => voice.name === 'Voice1') || voices[0];
		window.speechSynthesis.speak(utterance1);

		setTimeout(() => {
			const utterance2 = new SpeechSynthesisUtterance(word);
			utterance2.voice = voices.find((voice) => voice.name === 'Voice2') || voices[1];
			window.speechSynthesis.speak(utterance2);
		}, 2000);
	}
</script>

<svelte:head>
	<title>About</title>
	<meta name="description" content="About this app" />
</svelte:head>

<div>
	<h1>Typing Game</h1>
	{#if !gameStarted}
		<button on:click={() => setRoundDurationAndStart(30)}>30 Seconds</button>
		<button on:click={() => setRoundDurationAndStart(60)}>1 Minute</button>
	{/if}
	{#if gameStarted}
		<p>Round: {round}</p>
		<p>Word: {getMaskedWord(currentWord, round)}</p>
		<input type="text" bind:value={userInput} on:input={checkInput} disabled={timeLeft <= 0} />
		<p>{message}</p>
		<p>Total Trophies: {trophies}</p>
		<p>Time Left: {Math.floor(timeLeft / 60)}:{timeLeft % 60 < 10 ? '0' : ''}{timeLeft % 60}</p>
		<div>{positiveEmojis}</div>
		<button on:click={restartGame}>Restart Game</button>
	{/if}
</div>

<style>
	/* Add your styles here */
</style>
