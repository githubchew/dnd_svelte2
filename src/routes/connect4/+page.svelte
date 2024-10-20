<script>
	import { onMount } from 'svelte';

	let canvas;
	const rows = 6;
	const cols = 7;
	const cellSize = 100;
	const board = Array.from({ length: rows }, () => Array(cols).fill(null));
	const words = [
		'apple',
		'banana',
		'cherry',
		'date',
		'fig',
		'grape',
		'kiwi',
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
	const wordBoard = Array.from({ length: rows }, () => Array(cols).fill(null));
	let currentPlayer = 'red';
	let winner = null;
	let indicatorCol = 0; // New variable to track the column of the indicator
	let winningSequence = []; // New variable to store the winning sequence
	let winningColors = ['green', 'pink', 'gold'];
	let currentWinningColorIndex = 0;
	let colorInterval;

	function drawBoard(ctx) {
		ctx.clearRect(0, 0, canvas.width, canvas.height);
		ctx.fillStyle = 'blue';
		ctx.fillRect(0, 0, canvas.width, canvas.height);

		// Draw the indicator circle
		ctx.beginPath();
		ctx.arc(indicatorCol * cellSize + cellSize / 2, cellSize / 2, cellSize / 2 - 5, 0, 2 * Math.PI);
		ctx.fillStyle = currentPlayer;
		ctx.fill();
		ctx.closePath();

		for (let row = 0; row < rows; row++) {
			for (let col = 0; col < cols; col++) {
				ctx.beginPath();
				ctx.arc(
					col * cellSize + cellSize / 2,
					(row + 1) * cellSize + cellSize / 2, // Adjusted for the indicator row
					cellSize / 2 - 5,
					0,
					2 * Math.PI
				);

				// Check if the current cell is part of the winning sequence
				const isWinningCell = winningSequence.some(([r, c]) => r === row && c === col);

				// Determine the fill color
				let fillColor;
				if (isWinningCell) {
					fillColor =
						currentWinningColorIndex === winningColors.length
							? board[row][col] // Original color (red or yellow)
							: winningColors[currentWinningColorIndex];
				} else {
					fillColor = board[row][col] || 'white';
				}

				ctx.fillStyle = fillColor;
				ctx.fill();
				ctx.closePath();

				// Add a border to the winning circles
				if (isWinningCell) {
					ctx.strokeStyle = board[row][col]; // Use the original color for the border
					ctx.lineWidth = 3; // Set the border width
					ctx.stroke();
				}

				// Draw the word only if the cell is occupied
				if (board[row][col]) {
					if (!wordBoard[row][col]) {
						wordBoard[row][col] = words[Math.floor(Math.random() * words.length)];
					}
					ctx.fillStyle = 'black';
					ctx.font = 'bold 18px Arial'; // Increase font size to 18px
					ctx.textAlign = 'center';
					ctx.fillText(
						wordBoard[row][col],
						col * cellSize + cellSize / 2,
						(row + 1) * cellSize + cellSize / 2 + 4
					);
				}
			}
		}
	}

	function handleMouseMove(event) {
		const rect = canvas.getBoundingClientRect();
		const x = event.clientX - rect.left;
		indicatorCol = Math.floor(x / cellSize);
		drawBoard(canvas.getContext('2d'));
	}

	function handleClick(event) {
		if (winner) return; // Stop if there's already a winner

		const rect = canvas.getBoundingClientRect();
		const x = event.clientX - rect.left;
		const col = Math.floor(x / cellSize);

		for (let row = rows - 1; row >= 0; row--) {
			if (!board[row][col]) {
				animateDrop(row, col);
				break;
			}
		}
	}

	function animateDrop(targetRow, col) {
		let currentRow = 0;
		const ctx = canvas.getContext('2d');
		let bounceHeight = 10; // Height of the bounce
		let bounceCount = 0; // Number of bounces
		let isBouncing = false; // Flag to check if bouncing

		function drop() {
			if (currentRow <= targetRow) {
				drawBoard(ctx);
				ctx.beginPath();
				ctx.arc(
					col * cellSize + cellSize / 2,
					(currentRow + 1) * cellSize + cellSize / 2,
					cellSize / 2 - 5,
					0,
					2 * Math.PI
				);
				ctx.fillStyle = currentPlayer;
				ctx.fill();
				ctx.closePath();
				currentRow++;
				requestAnimationFrame(drop);
			} else if (!isBouncing) {
				isBouncing = true;
				bounce();
			}
		}

		function bounce() {
			if (bounceCount < 3) {
				// Number of bounces
				drawBoard(ctx);
				ctx.beginPath();
				ctx.arc(
					col * cellSize + cellSize / 2,
					(targetRow + 1) * cellSize + cellSize / 2 - bounceHeight,
					cellSize / 2 - 5,
					0,
					2 * Math.PI
				);
				ctx.fillStyle = currentPlayer;
				ctx.fill();
				ctx.closePath();
				bounceHeight = -bounceHeight / 2; // Reduce bounce height
				bounceCount++;
				setTimeout(bounce, 100); // Delay between bounces
			} else {
				board[targetRow][col] = currentPlayer;
				if (checkWin(targetRow, col)) {
					winner = currentPlayer;
					winningSequence = checkWin(targetRow, col); // Store the winning sequence
					startColorLoop(); // Start the color loop
				}
				currentPlayer = currentPlayer === 'red' ? 'yellow' : 'red';
				drawBoard(ctx);
			}
		}

		drop();
	}

	function checkWin(row, col) {
		return (
			checkDirection(row, col, 1, 0) || // Horizontal
			checkDirection(row, col, 0, 1) || // Vertical
			checkDirection(row, col, 1, 1) || // Diagonal /
			checkDirection(row, col, 1, -1) // Diagonal \
		);
	}

	function checkDirection(row, col, rowDir, colDir) {
		let count = 0;
		let sequence = [];
		for (let i = -3; i <= 3; i++) {
			const r = row + i * rowDir;
			const c = col + i * colDir;
			if (r >= 0 && r < rows && c >= 0 && c < cols && board[r][c] === currentPlayer) {
				count++;
				sequence.push([r, c]);
				if (count === 4) return sequence; // Return the winning sequence
			} else {
				count = 0;
				sequence = [];
			}
		}
		return null;
	}

	function startColorLoop() {
		colorInterval = setInterval(() => {
			currentWinningColorIndex = (currentWinningColorIndex + 1) % (winningColors.length + 1);
			drawBoard(canvas.getContext('2d'));
		}, 1000); // Change color every second
	}

	function stopColorLoop() {
		clearInterval(colorInterval);
	}

	function resetGame() {
		board.forEach((row) => row.fill(null));
		wordBoard.forEach((row) => row.fill(null));
		currentPlayer = 'red';
		winner = null;
		winningSequence = [];
		currentWinningColorIndex = 0;
		stopColorLoop();
		drawBoard(canvas.getContext('2d'));
	}

	onMount(() => {
		canvas.width = cols * cellSize;
		canvas.height = (rows + 1) * cellSize; // Adjusted for the indicator row
		const ctx = canvas.getContext('2d');
		drawBoard(ctx);
		return () => stopColorLoop(); // Cleanup interval on component unmount
	});
</script>

<canvas bind:this={canvas} on:click={handleClick} on:mousemove={handleMouseMove}></canvas>

{#if winner}
	<div class="winner-message">
		{winner} wins!
	</div>
{/if}

<button on:click={resetGame}>New Game</button>

<style>
	canvas {
		border: 1px solid black;
		display: block;
		margin: 0 auto;
	}
	.winner-message {
		text-align: center;
		font-size: 2em;
		color: green;
		margin-top: 20px;
	}
</style>
