<script>
	import { onMount } from 'svelte';

	let dinoSize = 50;
	let dinoY = 0;
	let dinoX = 0; // New variable to track dino's horizontal position
	let isJumping = false;
	let gravity = 0.2; // Adjust gravity for a balanced descent
	let jumpVelocity = 0;
	let jumpStrength = 10; // Increase jump strength for a faster ascent
	let obstacles = [];
	let collectibles = [];
	let gameSpeed = 1.5;
	let gameOver = false;
	let score = 0;
	let collectedEmojis = {}; // Object to store counts of collected emojis

	const emojis = [
		'📏ruler',
		'🍕pizza',
		'🐶dog',
		'🐱cat',
		'🐭mouse',
		'🐹',
		'🐰rabbit',
		'🦊fox',
		'🐻bear',
		'🐼panda',
		'🐨koala',
		'🐯tiger',
		'☃️snowman',
		'🪁kite',
		'🎈balloon',
		'🎁gift',
		'🎂cake',
		'🍬lollipop',
		'🍫chocolate',
		'🍭candy',
		'🧸teddybear'
	]; // Array of random emojis
	const monsterEmojis = ['👹', '👺', '👻', '💀', '👽', '🤖', '💥⚡👹👿']; // Array of monster emojis

	const heartEmoji = '❤️'; // Heart emoji for health items

	let health = 3; // Initialize health
	let isBackgroundRed = false; // Track if background should be red

	// Track which monster emojis have been collided with
	let collidedMonsters = new Set();

	let isFrozen = false; // New variable to track if dino is frozen

	let showSadFace = false; // New variable to control sad face display

	let lastCollectibleX = 0; // Track the x position of the last collectible

	function handleHealthDecrease() {
		showSadFace = true;
		isBackgroundRed = true; // Darken the background
		setTimeout(() => {
			showSadFace = false;
			isBackgroundRed = false; // Reset background color
		}, 1000);
	}

	function handleKeyPress(event) {
		if (event.key === 'x') {
			dinoSize = 30;
			dinoX += 100; // Move dino 100px to the right
			setTimeout(() => {
				dinoSize = 50;
				dinoX -= 100; // Move dino back to original position
			}, 400);
		}
		if (event.key === ' ' || event.key === 'z') {
			// Check for spacebar or 'z' key
			jump();
		}
	}

	function jump() {
		if (!isJumping) {
			isJumping = true;
			jumpVelocity = jumpStrength;
		}
	}

	function updateGame() {
		if (gameOver) return;

		// Update Dino position
		if (!isFrozen && isJumping) {
			dinoY += jumpVelocity;
			jumpVelocity -= gravity; // Apply gravity
			if (dinoY <= 0) {
				dinoY = 0;
				isJumping = false;
			}
		}

		// Move obstacles
		obstacles = obstacles
			.map((obstacle) => ({
				...obstacle,
				x: obstacle.x - gameSpeed
			}))
			.filter((obstacle) => obstacle.x > -20);

		// Ensure only one monster obstacle at a time
		if (obstacles.length === 0 && Math.random() < 0.02) {
			const monsterEmoji = monsterEmojis[Math.floor(Math.random() * monsterEmojis.length)];
			obstacles.push({ x: 400, y: 20, width: 220, height: 10, emoji: monsterEmoji });
		}

		// Move collectibles
		collectibles = collectibles
			.map((collectible) => ({
				...collectible,
				x: collectible.x - gameSpeed
			}))
			.filter((collectible) => collectible.x > -20);

		// Add new collectible randomly
		if (
			Math.random() < 0.01 &&
			(collectibles.length === 0 ||
				collectibles[collectibles.length - 1].x < lastCollectibleX - 100)
		) {
			const yPosition = Math.random() < 0.5 ? 20 : 150;
			const emoji =
				Math.random() < 0.1 ? heartEmoji : emojis[Math.floor(Math.random() * emojis.length)];
			const newCollectible = { x: 700, y: yPosition, width: 20, height: 20, emoji };
			collectibles.push(newCollectible);
			lastCollectibleX = newCollectible.x; // Update the last collectible x position
		}

		// Check for collisions with obstacles
		for (let obstacle of obstacles) {
			const dinoBottom = dinoY;
			const dinoTop = dinoY + dinoSize;
			const dinoLeft = 0;
			const dinoRight = dinoSize;

			const obstacleBottom = obstacle.y;
			const obstacleTop = obstacle.y + obstacle.height;
			const obstacleLeft = obstacle.x;
			const obstacleRight = obstacle.x + obstacle.width;

			if (
				dinoRight > obstacleLeft &&
				dinoLeft < obstacleRight &&
				dinoTop > obstacleBottom &&
				dinoBottom < obstacleTop
			) {
				if (!collidedMonsters.has(obstacle.emoji)) {
					health = Math.max(0, health - 1); // Decrease health, ensure it doesn't go below 0
					handleHealthDecrease(); // Call function to show sad face
					isBackgroundRed = true;
					setTimeout(() => {
						isBackgroundRed = false;
					}, 300);
					collidedMonsters.add(obstacle.emoji);
					isFrozen = true;
					setTimeout(() => {
						isFrozen = false;
					}, 300);
				}
				// Check game over condition after health update
				if (health <= 0) {
					gameOver = true;
				}
			}
		}

		// Check for collisions with collectibles
		collectibles = collectibles.filter((collectible) => {
			const dinoBottom = dinoY;
			const dinoTop = dinoY + dinoSize;
			const dinoLeft = 0;
			const dinoRight = dinoSize;

			const collectibleBottom = collectible.y;
			const collectibleTop = collectible.y + collectible.height;
			const collectibleLeft = collectible.x;
			const collectibleRight = collectible.x + collectible.width;

			if (
				dinoRight > collectibleLeft &&
				dinoLeft < collectibleRight &&
				dinoTop > collectibleBottom &&
				dinoBottom < collectibleTop
			) {
				if (collectible.emoji === heartEmoji) {
					health = Math.min(health + 1, 3); // Increase health, max 3
				} else {
					score += 10;
					collectedEmojis[collectible.emoji] = (collectedEmojis[collectible.emoji] || 0) + 1;
				}
				return false; // Remove collectible
			}
			return true;
		});

		requestAnimationFrame(updateGame);
	}

	// Reset collidedMonsters when obstacles are cleared
	function resetCollisions() {
		collidedMonsters.clear();
	}

	onMount(() => {
		window.addEventListener('keydown', handleKeyPress);
		updateGame();
	});
</script>

<!-- Remove the audio element for the collectible sound -->
<!-- <audio id="collectible-sound" src="/retro-coin-1.mp3"></audio> -->

<div class="game-container {isBackgroundRed ? 'red-background dark-background' : ''}">
	<!-- Display sad face emoji when health decreases -->
	{#if showSadFace}
		<div class="sad-face shake">😢</div>
	{/if}

	<div
		class="dino {isJumping ? 'jumping' : ''}"
		style="--dino-size: {dinoSize}px; transform: translate({dinoX}px, -{dinoY}px);"
	>
		<div class="eye left-eye"></div>
		<div class="eye right-eye"></div>
		<div class="mouth"></div>
		<!-- Add a div for the mouth -->
	</div>
	{#each obstacles as obstacle}
		<div
			class={obstacle.emoji === '💥⚡👹👿' ? 'obstacle special' : 'obstacle'}
			style="left: {obstacle.x}px; height: {obstacle.height}px; bottom: {obstacle.y}px;"
		>
			{obstacle.emoji}
		</div>
	{/each}
	{#each collectibles as collectible}
		<div class="collectible" style="left: {collectible.x}px; bottom: {collectible.y}px;">
			{collectible.emoji}
		</div>
	{/each}
</div>

{#if gameOver}
	<div>Game Over</div>
{/if}

<div>Score: {score}</div>
<div>
	Health: {#each Array(health) as _}{heartEmoji}{/each}
</div>
<!-- Display health as heart emojis -->
<div class="collected-emojis">
	{#each Object.entries(collectedEmojis) as [emoji, count]}
		<div>{emoji}: {count}</div>
	{/each}
</div>

<!-- Display collected emojis and their counts -->

<style>
	.game-container {
		position: relative;
		width: 800px;
		height: 400px;
		overflow: hidden;
		border: 1px solid black;
		background-image: url('https://img.freepik.com/free-vector/background-scene-wtih-blue-sky-green-grass_1308-101501.jpg?t=st=1728634920~exp=1728638520~hmac=445fbabc760de676cd6a191b5d70fafa86b1fab88a2300873591c31fc7930ca0&w=2000');
		background-size: cover;
		background-position: center;
		z-index: -1;
		transition: filter 0.5s ease; /* Add transition for smooth effect */
	}

	.game-container::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: inherit; /* Use the same background image */
		background-size: inherit;
		background-position: inherit;
		filter: blur(1px); /* Apply blur effect */
		z-index: -1; /* Ensure the overlay is behind other content */
	}

	.red-background {
		background-color: red;
		z-index: -1;
	}

	.dark-background {
		filter: brightness(0.5); /* Darken the background */
	}

	.dino {
		position: absolute;
		bottom: 0;
		width: var(--dino-size);
		height: var(--dino-size);
		background-color: green;
		transition:
			width 0.3s,
			height 0.3s,
			transform 0.4s; /* Add transform transition */
		display: flex;
		justify-content: center;
		align-items: center;
		z-index: 1; /* Ensure dino is above the background */
	}

	.eye {
		position: absolute;
		width: 10px;
		height: 10px;
		background-color: black;
		border-radius: 50%;
		animation: blink 4s infinite; /* Adjust the duration to 4 seconds */
	}

	.left-eye {
		left: 15px; /* Adjusted to ensure no overlap */
	}

	.right-eye {
		left: 35px; /* Adjusted to ensure no overlap */
	}

	.jumping .eye {
		width: 20px; /* Double the size */
		height: 20px; /* Double the size */
		transition:
			width 0.3s,
			height 0.3s,
			left 0.3s; /* Smooth transition */
	}

	.jumping .left-eye {
		left: 25px; /* Move further to the right */
	}

	.jumping .right-eye {
		left: 55px; /* Move further to the right */
	}

	.mouth {
		position: absolute;
		bottom: -6px; /* Position the mouth */
		width: 30px;
		height: 10px;
		background-color: black;
		border-radius: 0px 0px 10 10px; /* Create a bottom half-circle */
	}

	@keyframes blink {
		0%,
		5%,
		95%,
		100% {
			transform: scaleY(1);
		}
		2.5%,
		97.5% {
			transform: scaleY(0.1);
		}
	}

	.obstacle {
		position: absolute;
		bottom: 0;
		width: 20px;
		height: 300px;
		font-size: 30px; /* Increase font size for monster emoji */
		display: flex;
		align-items: center;
		justify-content: center;
		transform: translateY(-10px); /* Move monster emoji higher */
		z-index: 2; /* Ensure obstacles are above the pseudo-element */
	}

	.obstacle.special {
		transform: translateY(-100px); /* Move special monster emoji one-third higher */
	}

	.obstacle::before {
		content: '';
		position: absolute;
		width: 40px;
		height: 40px;
		border-radius: 50%;
		background-color: rgba(244, 121, 121, 0.5);
		animation: pulse 1.5s infinite;
		z-index: 1; /* Ensure pseudo-element is below the emoji */
	}

	.air-obstacle {
		position: absolute;
		width: 20px;
		height: 20px;
		font-size: 30px; /* Increase font size for monster emoji */
		transform: translateY(-10px); /* Move monster emoji higher */
	}

	.collectible {
		position: absolute;
		width: 20px;
		height: 20px;
		font-size: 30px; /* Increase font size for other emojis */
		z-index: 1; /* Ensure collectibles are above the background */
	}

	.collected-emojis {
		margin-top: 10px;
		font-size: 20px;
	}

	@keyframes pulse {
		0% {
			transform: scale(1);
			opacity: 0.5;
		}
		50% {
			transform: scale(1.2);
			opacity: 1;
		}
		100% {
			transform: scale(1);
			opacity: 0.5;
		}
	}

	.sad-face {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		font-size: 100px; /* Adjust size as needed */
		z-index: 10; /* Ensure it appears above other elements */
	}

	.shake {
		animation: shake 0.5s; /* Duration of the shake effect */
		animation-iteration-count: infinite; /* Repeat the animation */
	}

	@keyframes shake {
		0%,
		100% {
			transform: translate(-50%, -50%);
		}
		10%,
		30%,
		50%,
		70%,
		90% {
			transform: translate(-48%, -50%);
		}
		20%,
		40%,
		60%,
		80% {
			transform: translate(-52%, -50%);
		}
	}
</style>