<script>
	let quizData = [
		{
			question: 'The capital of France is _______.',
			answer: 'Paris',
			hint: 'City of love and lights'
		},
		{
			question: 'The largest planet in our solar system is _______.',
			answer: 'Jupiter',
			hint: 'Gas giant'
		},
		{
			question: 'The famous painting "The Starry Night" was created by _______.',
			answer: 'Vincent van Gogh',
			hint: 'Dutch post-impressionist artist'
		}
	];

	let currentQuestion = 0;
	let userAnswer = '';
	let showAnswer = false;
	let correct = false;
	let score = 0;

	function submitAnswer() {
		if (userAnswer.toLowerCase() === quizData[currentQuestion].answer.toLowerCase()) {
			correct = true;
			score++;
		} else {
			correct = false;
		}
		showAnswer = true;
	}

	function nextQuestion() {
		userAnswer = '';
		showAnswer = false;
		correct = false;
		currentQuestion++;
		if (currentQuestion >= quizData.length) {
			currentQuestion = 0;
		}
	}

	function shuffleQuestions() {
		quizData = quizData.map((a) => ({ ...a })).sort(() => Math.random() - 0.5);
		currentQuestion = 0;
		score = 0;
	}

	function handleKeyPress(event) {
		if (event.key === 'Enter') {
			submitAnswer();
		}
	}
</script>

<div>
	<h1>Fill-in-the-blank Quiz</h1>
	<p>Score: {score}/{quizData.length}</p>
	<p>{quizData[currentQuestion].question}</p>
	<input type="text" bind:value={userAnswer} on:keypress={handleKeyPress} />
	<button on:click={submitAnswer}>Submit</button>
	<p>Hint: {quizData[currentQuestion].hint}</p>
	{#if showAnswer}
		{#if correct}
			<p>Correct! The answer is {quizData[currentQuestion].answer}</p>
		{:else}
			<p>Sorry, the correct answer is {quizData[currentQuestion].answer}</p>
		{/if}
		<button on:click={nextQuestion}>Next Question</button>
	{/if}
	{#if currentQuestion === 0 && score > 0}
		<button on:click={shuffleQuestions}>Shuffle Questions</button>
	{/if}
</div>
