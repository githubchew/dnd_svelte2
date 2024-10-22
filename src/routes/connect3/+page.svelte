<script>
	import { onMount } from 'svelte';

	let canvas;
	const rows = 4;
	const cols = 6;
	const cellSize = 100;
	const scaleFactor = 2;
	const board = Array.from({ length: rows }, () => Array(cols).fill(null));
	const words = [
		'🍎 apple',
		'🍌 banana',
		'🍒 cherry',
		'🌴 date',
		'🍈 fig',
		'🍇 grape',
		'🥝 kiwi',
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
		'🍎 apple',
		'👶 baby',
		'🔙 back',
		'⚽ ball',
		'🐻 bear',
		'🛏️ bed',
		'🔔 bell',
		'🐦 bird',
		'🎂 birthday',
		'⛵ boat',
		'📦 box',
		'👦 boy',
		'🍞 bread',
		'👨‍👦 brother',
		'🍰 cake',
		'🚗 car',
		'🐱 cat',
		'🪑 chair',
		'🐔 chicken',
		'👶 children',
		'🎄 Christmas',
		'🧥 coat',
		'🌽 corn',
		'🐄 cow',
		'📅 day',
		'🐕 dog',
		'🪆 doll',
		'🚪 door',
		'🦆 duck',
		'🥚 egg',
		'👁️ eye',
		'🌾 farm',
		'👨‍🌾 farmer',
		'👨‍👦 father',
		'👣 feet',
		'🔥 fire',
		'🐟 fish',
		'🪜 floor',
		'🌸 flower',
		'🎮 game',
		'🌻 garden',
		'👧 girl',
		'👋 goodbye',
		'🌿 grass',
		'🌍 ground',
		'✋ hand',
		'🗣️ head',
		'⛰️ hill',
		'🏠 home',
		'🐎 horse',
		'🏡 house',
		'🐱 kitty',
		'🦵 leg',
		'✉️ letter',
		'👨 man',
		'👨‍👨‍👦 men',
		'🥛 milk',
		'💰 money',
		'🌅 morning',
		'👩‍👦 mother',
		'📛 name',
		'🪺 nest',
		'🌃 night',
		'📄 paper',
		'🎉 party',
		'🖼️ picture',
		'🐖 pig',
		'🐇 rabbit',
		'🌧️ rain',
		'💍 ring',
		'🐦 robin',
		'🎅 Santa Claus',
		'🏫 school',
		'🌱 seed',
		'🐑 sheep',
		'👞 shoe',
		'👧 sister',
		'❄️ snow',
		'🎵 song',
		'🐿️ squirrel',
		'🪵 stick',
		'🛣️ street',
		'☀️ sun',
		'🪑 table',
		'🔨 thing',
		'⏰ time',
		'🔝 top',
		'🧸 toy',
		'🌳 tree',
		'⌚ watch',
		'💧 water',
		'🛣️ way',
		'🌬️ wind',
		'🪟 window',
		'🪵 wood',
	];
	const wordBoard = Array.from({ length: rows }, () => Array(cols).fill(null));
	let currentPlayer = 'red';
	let winner = null;
	let indicatorCol = 0;
	let winningSequence = [];
	let winningColors = ['green', 'pink', 'gold'];
	let currentWinningColorIndex = 0;
	let colorInterval;
	let emojiAnimationFrame;
	let emojiOffset = 0;
	let emojiDirection = 1;

	function drawBoard(ctx) {
		ctx.clearRect(0, 0, canvas.width, canvas.height);
		ctx.fillStyle = 'blue';
		ctx.fillRect(0, 0, canvas.width, canvas.height);

		ctx.strokeStyle = currentPlayer;
		ctx.lineWidth = 2;
		ctx.strokeRect(indicatorCol * cellSize, cellSize, cellSize, rows * cellSize);

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
					(row + 1) * cellSize + cellSize / 2,
					cellSize / 2 - 5,
					0,
					2 * Math.PI
				);

				const isWinningCell = winningSequence.some(([r, c]) => r === row && c === col);
				let fillColor = isWinningCell
					? (currentWinningColorIndex === winningColors.length ? board[row][col] : winningColors[currentWinningColorIndex])
					: (board[row][col] || 'white');

				ctx.fillStyle = fillColor;
				ctx.fill();
				ctx.closePath();

				if (isWinningCell) {
					ctx.strokeStyle = board[row][col];
					ctx.lineWidth = 3;
					ctx.stroke();

					ctx.fillStyle = 'black';
					ctx.font = 'bold 24px Arial';
					ctx.textAlign = 'right';
					ctx.fillText(
						'👍',
						col * cellSize + cellSize - 10,
						(row + 1) * cellSize + 10 + emojiOffset
					);
				}

				if (board[row][col]) {
					if (!wordBoard[row][col]) {
						wordBoard[row][col] = words[Math.floor(Math.random() * words.length)];
					}
					ctx.fillStyle = 'black';
					ctx.font = 'bold 22px Arial';
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
		if (winner) return;

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
		let bounceHeight = 10;
		let bounceCount = 0;
		let isBouncing = false;

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
				bounceHeight = -bounceHeight / 2;
				bounceCount++;
				setTimeout(bounce, 100);
			} else {
				board[targetRow][col] = currentPlayer;
				if (checkWin(targetRow, col)) {
					winner = currentPlayer;
					winningSequence = checkWin(targetRow, col);
					startColorLoop();
				}
				currentPlayer = currentPlayer === 'red' ? 'yellow' : 'red';
				drawBoard(ctx);
			}
		}

		drop();
	}

	function checkWin(row, col) {
		return (
			checkDirection(row, col, 1, 0) ||
			checkDirection(row, col, 0, 1) ||
			checkDirection(row, col, 1, 1) ||
			checkDirection(row, col, 1, -1)
		);
	}

	function checkDirection(row, col, rowDir, colDir) {
		let count = 0;
		let sequence = [];
		for (let i = -2; i <= 2; i++) {
			const r = row + i * rowDir;
			const c = col + i * colDir;
			if (r >= 0 && r < rows && c >= 0 && c < cols && board[r][c] === currentPlayer) {
				count++;
				sequence.push([r, c]);
				if (count === 3) return sequence;
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
		}, 1000);
	}

	function stopColorLoop() {
		clearInterval(colorInterval);
	}

	function animateEmojis() {
		emojiOffset += emojiDirection * 1; // Slower increment
		if (emojiOffset > 5 || emojiOffset < -5) {
			emojiDirection *= -1;
		}
		drawBoard(canvas.getContext('2d'));
		emojiAnimationFrame = requestAnimationFrame(() => setTimeout(animateEmojis, 50)); // Add delay
	}

	function startEmojiAnimation() {
		if (!emojiAnimationFrame) {
			animateEmojis();
		}
	}

	function stopEmojiAnimation() {
		cancelAnimationFrame(emojiAnimationFrame);
		emojiAnimationFrame = null;
	}

	function resetGame() {
		board.forEach((row) => row.fill(null));
		wordBoard.forEach((row) => row.fill(null));
		currentPlayer = 'red';
		winner = null;
		winningSequence = [];
		currentWinningColorIndex = 0;
		stopColorLoop();
		stopEmojiAnimation();
		drawBoard(canvas.getContext('2d'));
	}

	onMount(() => {
		canvas.width = cols * cellSize * scaleFactor;
		canvas.height = (rows + 1) * cellSize * scaleFactor;
		
		canvas.style.width = `${cols * cellSize}px`;
		canvas.style.height = `${(rows + 1) * cellSize}px`;

		const ctx = canvas.getContext('2d');
		ctx.scale(scaleFactor, scaleFactor);
		drawBoard(ctx);
		startEmojiAnimation();
		return () => {
			stopColorLoop();
			stopEmojiAnimation();
		};
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

