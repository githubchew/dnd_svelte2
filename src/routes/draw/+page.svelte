<script>
	let pixelSize = 25; // 400px / 16 = 25px per pixel
	let drawing = false;
	let color = '#000000'; // default color is black
	let brushSize = 1; // default brush size is 1 pixel
	let randomColors = [getRandomColor(), getRandomColor(), getRandomColor()];

	function handleMouseDown(event) {
		drawing = true;
		drawPixel(event.offsetX, event.offsetY);
	}

	function handleMouseMove(event) {
		if (drawing) {
			drawPixel(event.offsetX, event.offsetY);
		}
	}

	function handleMouseUp() {
		drawing = false;
	}

	function drawPixel(x, y) {
		const canvas = document.getElementById('canvas');
		const ctx = canvas.getContext('2d');
		const pixelX = Math.floor(x / pixelSize);
		const pixelY = Math.floor(y / pixelSize);
		ctx.fillStyle = color;
		ctx.fillRect(
			pixelX * pixelSize,
			pixelY * pixelSize,
			pixelSize * brushSize,
			pixelSize * brushSize
		);
	}

	function clearCanvas() {
		const canvas = document.getElementById('canvas');
		const ctx = canvas.getContext('2d');
		ctx.clearRect(0, 0, 400, 400);
		randomColors = [getRandomColor(), getRandomColor(), getRandomColor()];
	}

	function handleChangeColor(event) {
		color = event.target.value;
	}

	function handleChangeBrushSize(event) {
		brushSize = parseInt(event.target.value);
	}

	function handleSave() {
		const canvas = document.getElementById('canvas');
		const dataURL = canvas.toDataURL();
		const a = document.createElement('a');
		a.href = dataURL;
		a.download = 'etch-a-sketch.png';
		a.click();
	}

	function getRandomColor() {
		const letters = '0123456789ABCDEF';
		let color = '#';
		for (let i = 0; i < 6; i++) {
			color += letters[Math.floor(Math.random() * 16)];
		}
		return color;
	}
</script>

<div class="containerMain">
	<canvas
		id="canvas"
		width={400}
		height={400}
		on:mousedown={handleMouseDown}
		on:mousemove={handleMouseMove}
		on:mouseup={handleMouseUp}
		style="border: 1px solid black"
	/>
	<div class="buttonsColorOptions" style="margin-left: 20px;">
		<input type="color" value={color} on:input={handleChangeColor} />
		<button
			style={`background-color: #000000; border: none; padding: 10px; border-radius: 50%; cursor: pointer;`}
			on:click={() => (color = '#000000')}
		></button>
		{#each randomColors as randomColor}
			<button
				style={`background-color: ${randomColor}; border: none; padding: 10px; border-radius: 50%; cursor: pointer;`}
				on:click={() => (color = randomColor)}
			></button>
		{/each}
	</div>
	<input type="range" min="1" max="10" value={brushSize} on:input={handleChangeBrushSize} />
	<span>🖌️ Brush Size : {brushSize}</span>

	<div class="buttonsClearSave">
		<button on:click={clearCanvas}>Clear</button>
		<button on:click={handleSave}>Save</button>
	</div>
</div>

<style>
	@keyframes backgroundShape {
		0% {
			border-radius: 73% 27% 45% 55% / 30% 30% 70% 70%;
		}

		50% {
			border-radius: 73% 27% 55% 45% / 30% 30% 70% 70%;
		}

		100% {
			border-radius: 73% 27% 45% 55% / 30% 30% 70% 70%;
		}
	}
	.containerMain {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}
	canvas {
		background-color: white;
		cursor: crosshair;
	}

	.buttonsColorOptions {
		display: flex;
		flex-direction: row;
		width: 390px;
		/* border: 1px solid black; */
		justify-content: space-around;
		align-items: center;
		margin: 1rem;
	}

	.buttonsClearSave {
		display: flex;
		width: 200px;
		/* border: 1px solid green; */
		flex-direction: row;
		justify-content: space-between;
		margin: 1rem;
	}
	.buttonsClearSave button {
		background-color: rgb(218, 218, 218);
		width: 100px;
		margin: 0.5rem;
		border: none;
		border-radius: 0.5rem;
		font-size: 1.4rem;
	}
	.buttonsColorOptions button {
		width: 3rem;
		height: 3rem;
	}

	input {
		border: none;
		outline: none;
		background-color: #a8a8a8;
	}

	button:active {
		transform: scale(0.9);
	}
</style>
