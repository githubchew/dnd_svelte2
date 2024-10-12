<script>
	import { onMount } from 'svelte';

	let canvas;
	let ctx;
	let dinoSize = 50;
	let dinoY = 0;
	let dinoX = 0;
	let isJumping = false;
	let gravity = 0.2;
	let jumpVelocity = 0;
	let jumpStrength = 10;
	let obstacles = [];
	let collectibles = [];
	let gameSpeed = 1.5;
	let gameOver = false;
	let score = 0;
	let collectedEmojis = {};
	let health = 3;
	let isBackgroundRed = false;
	let collidedMonsters = new Set();
	let showSadFace = false;
	let lastCollectibleX = 0;
	let eyesOpen = true;
	let shakeTimer = 0;
	let smile = false;
	let brightness = 1;
	let hurtState = false;
	let heartEyes = false;

	const emojis = [
		'📏ruler', '🍕pizza', '🐶', '🐱', '🐭mouse', '🐹', '🐰rabbit', '🦊', '🐻', '🐼', '🐨', '🐯', '☃️', '🪁', '🎈balloon', '🎁gift', '🎂', '🍬', '🍫', '🍭', '🧸'
	];
	const monsterEmojis = ['👹', '👺', '👻', '💀', '👽', '🤖', '💥⚡👹👿'];
	const heartEmoji = '❤️';

	function handleHealthDecrease() {
		showSadFace = true;
		isBackgroundRed = true;
		setTimeout(() => {
			showSadFace = false;
			isBackgroundRed = false;
		}, 1000);
	}

	function handleKeyPress(event) {
		if (event.key === 'x') {
			dinoSize = 30;
			dinoX += 100;
			setTimeout(() => {
				dinoSize = 50;
				dinoX -= 100;
			}, 400);
		}
		if (event.key === ' ' || event.key === 'z') {
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

		// Clear the canvas
		ctx.clearRect(0, 0, canvas.width, canvas.height);

		// Update Dino position, scale, and rotation
		let dinoScaleY = 1; // Default scale
		let dinoRotation = 0; // Default rotation

		if (isJumping) {
			dinoY += jumpVelocity;
			jumpVelocity -= gravity;

			// Scale up when starting the jump
			if (jumpVelocity > 0) {
				dinoScaleY = 1.2;
				dinoRotation = -Math.PI / 40; // Rotate 22.5 degrees
			}
			// Scale back to normal at the peak
			else if (dinoY <= 0) {
				dinoY = 0;
				isJumping = false;
				dinoScaleY = 1;
				dinoRotation = 0;
			}
		}

		// Calculate eye position and size
		let eyeOffsetX = 0;
		let eyeScale = isJumping ? 1.5 : 1; // Eyes are bigger when jumping
		let nearestEmojiDistance = Infinity;

		collectibles.forEach(collectible => {
			const distance = Math.abs(collectible.x - dinoX);
			if (distance < nearestEmojiDistance) {
				nearestEmojiDistance = distance;
			}
		});

		if (nearestEmojiDistance < 100) {
			eyeOffsetX = 10; // Increase the offset to move eyes further to the right
		}

		// Update shake effect
		let shakeScale = 1;
		let shakeRotation = 0;
		if (shakeTimer > 0) {
			shakeScale = 1 + Math.sin(shakeTimer * 0.1) * 0.2; // More dramatic scaling
			shakeRotation = Math.sin(shakeTimer * 0.2) * 0.3; // More dramatic rotation
			shakeTimer--;
		}

		// Draw Dino with scaling and rotation effect
		ctx.save();
		ctx.translate(dinoX + dinoSize / 2, canvas.height - dinoY - dinoSize / 2);
		ctx.scale(1, dinoScaleY); // Apply Y scaling
		ctx.scale(shakeScale, shakeScale);
		ctx.rotate(dinoRotation + shakeRotation); // Apply rotation
		ctx.translate(-dinoSize / 2, -dinoSize / 2);
		ctx.fillStyle = 'green';
		ctx.fillRect(0, 0, dinoSize, dinoSize);

		// Draw Dino Eyes or Heart Eyes
		if (heartEyes) {
			ctx.font = '20px "Segoe UI Emoji", "Apple Color Emoji"';
			ctx.fillText('❤️', 10 + eyeOffsetX, 20);
			ctx.fillText('❤️', 30 + eyeOffsetX, 20);
		} else {
			ctx.fillStyle = 'black';
			const eyeRadius = 5 * (hurtState ? 3 : eyeScale); // Eyes are three times bigger when hurt
			const eyeScaleY = eyesOpen ? 1 : 0.2; // Scale down on y-axis when eyes are closed

			// Left Eye
			ctx.save();
			ctx.translate(15 + eyeOffsetX, 15);
			ctx.scale(1, eyeScaleY);
			ctx.beginPath();
			ctx.arc(0, 0, eyeRadius, 0, Math.PI * 2);
			ctx.fill();
			ctx.restore();

			// Right Eye
			ctx.save();
			ctx.translate(35 + eyeOffsetX, 15);
			ctx.scale(1, eyeScaleY);
			ctx.beginPath();
			ctx.arc(0, 0, eyeRadius, 0, Math.PI * 2);
			ctx.fill();
			ctx.restore();
		}

		// Draw Frown
		if (hurtState) {
			ctx.strokeStyle = 'black';
			ctx.lineWidth = 2;
			ctx.beginPath();
			ctx.arc(25, 40, 10, 0, Math.PI, true); // Frown
			ctx.stroke();
		} else if (smile) {
			ctx.strokeStyle = 'black';
			ctx.lineWidth = 2;
			ctx.beginPath();
			ctx.arc(25, 35, 10, 0, Math.PI, false); // Smile
			ctx.stroke();
		}

		ctx.restore();

		// Move and draw obstacles with flip effect
		obstacles = obstacles
			.map((obstacle) => ({
				...obstacle,
				x: obstacle.x - gameSpeed,
				flip: Math.sin(obstacle.x * 0.05) // Flip effect
			}))
			.filter((obstacle) => obstacle.x > -20);

		obstacles.forEach(obstacle => {
			ctx.save();
			ctx.translate(obstacle.x + 15, canvas.height - obstacle.y - 15);
			ctx.scale(obstacle.flip, 1); // Apply flip effect
			ctx.font = '40px "Segoe UI Emoji", "Apple Color Emoji"'; // Double size
			ctx.fillText(obstacle.emoji, -15, 15);
			ctx.restore();
		});

		// Move and draw collectibles
		collectibles = collectibles
			.map((collectible) => ({
				...collectible,
				x: collectible.x - gameSpeed
			}))
			.filter((collectible) => collectible.x > -20);

		collectibles.forEach(collectible => {
			ctx.font = '40px "Segoe UI Emoji", "Apple Color Emoji"'; // Double size
			ctx.fillText(collectible.emoji, collectible.x, canvas.height - collectible.y);
		});

		// Add new obstacles and collectibles
		if (obstacles.length === 0 && Math.random() < 0.02) {
			const monsterEmoji = monsterEmojis[Math.floor(Math.random() * monsterEmojis.length)];
			obstacles.push({ x: 400, y: 20, width: 220, height: 10, emoji: monsterEmoji });
		}

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
			lastCollectibleX = newCollectible.x;
		}

		// Check for collisions with obstacles
		for (let obstacle of obstacles) {
			const dinoBottom = dinoY;
			const dinoTop = dinoY + dinoSize;
			const dinoLeft = dinoX;
			const dinoRight = dinoX + dinoSize;

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
					health = Math.max(0, health - 1);
					handleHealthDecrease();
					isBackgroundRed = true;
					setTimeout(() => {
						isBackgroundRed = false;
					}, 300);
					collidedMonsters.add(obstacle.emoji);
					hurtState = true;
					shakeTimer = 30; // Start shaking
					brightness = 0.5; // Dim the brightness
					setTimeout(() => {
						hurtState = false;
						brightness = 1; // Reset brightness
					}, 300);
				}
				if (health <= 0) {
					gameOver = true;
				}
			}
		}

		// Check for collisions with collectibles
		collectibles = collectibles.filter((collectible) => {
			const dinoBottom = dinoY;
			const dinoTop = dinoY + dinoSize;
			const dinoLeft = dinoX;
			const dinoRight = dinoX + dinoSize;

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
					health = Math.min(health + 1, 3);
					heartEyes = true; // Set heart eyes
					setTimeout(() => {
						heartEyes = false; // Reset after a short time
					}, 1000);
				} else {
					score += 10;
					collectedEmojis[collectible.emoji] = (collectedEmojis[collectible.emoji] || 0) + 1;
					smile = true; // Make dino smile
					setTimeout(() => {
						smile = false; // Stop smiling after a short time
					}, 1000);
				}
				return false;
			}
			return true;
		});

		// Draw score and health
		ctx.fillStyle = 'black';
		ctx.font = '20px Arial';
		ctx.fillText(`Score: ${score}`, 10, 20);
		ctx.fillText(`Health: ${'❤️'.repeat(health)}`, 10, 40);

		// Draw sad face if health decreases
		if (showSadFace) {
			ctx.font = '100px Arial';
			ctx.fillText('😢', canvas.width / 2 - 50, canvas.height / 2);
		}

		// Draw game over message
		if (gameOver) {
			ctx.font = '50px Arial';
			ctx.fillText('Game Over', canvas.width / 2 - 150, canvas.height / 2);
		}

		requestAnimationFrame(updateGame);
	}

	onMount(() => {
		canvas = document.querySelector('canvas');
		ctx = canvas.getContext('2d');
		window.addEventListener('keydown', handleKeyPress);
		updateGame();

		// Toggle eyes open/closed every 2 seconds
		setInterval(() => {
			eyesOpen = !eyesOpen;
		}, 2000);
	});
</script>

<canvas width="800" height="400" style="filter: brightness({brightness})"></canvas>

<div class="emoji-counts">
	<h3>Collected Emojis:</h3>
	<ul>
		{#each Object.entries(collectedEmojis) as [emoji, count]}
			<li>{emoji}: {count}</li>
		{/each}
	</ul>
</div>

<style>
	canvas {
		border: 1px solid black;
		background-image: url('https://img.freepik.com/free-vector/background-scene-wtih-blue-sky-green-grass_1308-101501.jpg?t=st=1728634920~exp=1728638520~hmac=445fbabc760de676cd6a191b5d70fafa86b1fab88a2300873591c31fc7930ca0&w=2000');
		background-size: cover;
		background-position: center;
	}
	.emoji-counts {
		margin-top: 20px;
		font-size: 18px;
	}
</style>