<section class="main-container">
	<div class="boy-container">
		<div class="boy-container__letter">b</div>
		<div class="boy-container__emoji">🏃‍➡️</div>
	</div>

	<div class="dog-container">
		<div class="dog-container__letter">d</div>
		<div class="dog-container__emoji">🐕‍🦺</div>
	</div>
</section>

<section class="thumbs-up-container">
	<div>
		<div class="thumbs-up-container__letter">b</div>
		<div class="thumbs-up-container__emoji">👍</div>
	</div>
	<div>
		<div class="thumbs-up-container__letter">d</div>
		<div class="thumbs-up-container__emoji flip-emoji">👍</div>
	</div>
</section>

<style>
	.main-container {
		width: 20rem;
		font-size: 10rem;
		margin: 0 auto;
		display: flex;
		justify-content: space-around;
		align-items: center;
		flex-wrap: wrap; /* Allow items to wrap to the next line */
	}

	.boy-container,
	.dog-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		position: relative;
	}

	.boy-container__letter,
	.boy-container__emoji {
		position: absolute;
	}

	.boy-container__letter {
		z-index: -1;
	}

	.boy-container__emoji {
		z-index: 0;
		font-size: 5rem;
		padding-top: 4rem;
		animation: moveBoyEmoji 5s ease-out infinite; /* Total duration is 5s */
	}

	@keyframes moveBoyEmoji {
		0% {
			transform: translateX(-100%);
		}
		40% {
			transform: translateX(0);
		}
		100% {
			transform: translateX(0); /* Stay at final position */
		}
	}

	.dog-container__letter,
	.dog-container__emoji {
		position: absolute;
	}

	.dog-container__letter {
		z-index: -1;
	}

	.dog-container__emoji {
		z-index: 0;
		font-size: 4rem;
		padding-top: 4rem;
		padding-left: 1rem;
		animation: moveDogEmoji 5s ease-out infinite; /* Total duration is 5s */
	}

	@keyframes moveDogEmoji {
		0% {
			transform: translateX(100%);
		}
		40% {
			transform: translateX(0);
		}
		100% {
			transform: translateX(0); /* Stay at final position */
		}
	}

	.thumbs-up-container {
		font-size: 4rem;
		width: 20rem;
		position: absolute;
		left: 50%; /* Center horizontally */
		top: 70%; /* Center vertically */
		transform: translate(-50%, -50%); /* Adjust for the element's own size */
		display: flex;
		justify-content: space-around;
		align-items: center;
		text-align: center;
	}

	.flip-emoji {
		transform: scaleX(-1); /* Flips the element horizontally */
	}
</style>
