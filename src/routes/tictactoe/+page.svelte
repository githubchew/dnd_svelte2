<script>
	let board = Array(9).fill(null);
	let currentPlayer = 'X';
	let winner = null;
	let winningCombination = null;
	let words = Array.from(
		new Set([
			'dragon🐉',
			'unicorn🦄',
			'rocket🚀',
			'cactus🌵',
			'fire🔥',
			'moon🌙',
			'sun🌞',
			'star⭐',
			'cloud☁️',
			'rainbow🌈',
			'coffee☕',
			'book📚',
			'camera📷',
			'globe🌍',
			'heart❤️',
			'lightning⚡',
			'umbrella☂️',
			'anchor⚓',
			'crown👑',
			'gem💎',
			'volcano🌋',
			'earth🌎',
			'comet☄️',
			'robot🤖',
			'alien👽',
			'medal🏅',
			'trophy🏆',
			'medal🥇',
			'medal🥈',
			'medal🥉',
			'balloon🎈',
			'party🎉',
			'gift🎁',
			'clown🤡',
			'ghost👻',
			'jack-o-lantern🎃',
			'skull💀',
			'bat🦇',
			'owl🦉',
			'cat🐈',
			'dog🐕',
			'fox🦊',
			'butterfly🦋',
			'bee🐝',
			'bug🐛',
			'ant🐜',
			'beetle🐞',
			'snail🐌',
			'cricket🦗',
			'dragonfly🦟',
			'caterpillar🐛',
			'lobster🦞',
			'shrimp🦐',
			'squid🦑',
			'jellyfish🪼',
			'coral🪸',
			'seaweed🌿',
			'pearl🦪',
			'mermaid🧜‍♀️',
			'merman🧜‍♂️',
			'trident🔱',
			'pirate🏴‍☠️',
			'vampire🧛‍♂️',
			'zombie🧟‍♂️',
			'genie🧞‍♂️',
			'fairy🧚‍♀️',
			'elf🧝‍♂️',
			'centaur🧝‍♀️',
			'minotaur🧟‍♀️',
			'cyclops👁️',
			'phoenix🦅',
			'griffin🦅',
			'pegasus🦄',
			'kraken🦑',
			'leviathan🐉',
			'cerberus🐕',
			'hydra🐉',
			'medusa🐍',
			'harpy🦅',
			'sphinx🦁',
			'werewolf🐺',
			'loch ness monster🐉',
			'bigfoot🦶',
			'yeti🦶',
			'chupacabra🐐',
			'jackalope🐇',
			'desert🏜️',
			'oasis🏝️',
			'camel🐪',
			'caravan🏜️',
			'apple🍎',
			'banana🍌',
			'cherry🍒',
			'watermelon🍉',
			'strawberry🍓',
			'whale🐋',
			'dolphin 🐬',
			'grape🍇',
			'ice cream 🍦',
			'going home🏠',
			'going to school🏫',
			'chocolate🍫',
			'pizza🍕',
			'burger🍔',
			'hot dog🌭',
			'sandwich🥪',
			'cookie🍪',
			'cake🍰',
			'penguin🐧',
			'tiger🐯',
			'lion🦁',
			'elephant🐘',
			'monkey🐒',
			'bear🐻',
			'dog🐶',
			'cat🐱',
			'bird🐦',
			'horse🐴',
			'sheep🐑',
			'cow🐄',
			'pig🐷',
			'chicken🐔',
			'duck🦆',
			'turkey🦃',
			'goat🐐',
			'rabbit🐰',
			'wolf🐺',
			'turtle🐢',
			'snake🐍',
			'fish🐟',
			'octopus🐙',
			'crab🦀',
			'shark🦈',
			'seahorse🐴',
			'starfish🐟',
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
			'wood',
			'🧦socks',
			'👕shirt',
			'👠shoe',
			'👟sneaker',
			'👒hat',
			'🍕pizza',
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
			'🍬candy',
			'🍫chocolate',
			'🍭lollipop',
			'🧸teddybear',
			'🐉dragon',
			'🦄unicorn',
			'🚀rocket',
			'🌵cactus',
			'🔥fire',
			'🌙moon',
			'🌞sun',
			'⭐star',
			'☁️cloud',
			'🌈rainbow',
			'☕coffee',
			'📚book',
			'📷camera',
			'🌍globe',
			'❤️heart',
			'⚡lightning',
			'☂️umbrella',
			'⚓anchor',
			'👑crown',
			'💎gem',
			'🌋volcano',
			'🌎earth',
			'☄️comet',
			'🤖robot',
			'👽alien',
			'🏅medal',
			'🏆trophy',
			'🎉party',
			'🤡clown',
			'👻ghost',
			'🎃jack-o-lantern',
			'💀skull',
			'🦇bat',
			'🦉owl',
			'🐈cat',
			'🐕dog',
			'🦋butterfly',
			'🐝bee',
			'🐛bug',
			'🐜ant',
			'🐞beetle',
			'🐌snail',
			'🦗cricket',
			'🦟dragonfly',
			'🐛caterpillar',
			'🦞lobster',
			'🦐shrimp',
			'🦑squid',
			'🪼jellyfish',
			'🪸coral',
			'🌿seaweed',
			'🦪pearl',
			'🧜‍♀️mermaid',
			'🧜‍♂️merman',
			'🔱trident',
			'🏴‍☠️pirate',
			'🧛‍♂️vampire',
			'🧟‍♂️zombie',
			'🧞‍♂️genie',
			'🧚‍♀️fairy',
			'🧝‍♂️elf',
			'🧝‍♀️centaur',
			'🧟‍♀️minotaur',
			'👁️cyclops',
			'🦅phoenix',
			'🦅griffin',
			'🦄pegasus',
			'🦑kraken',
			'🐉leviathan',
			'🐕cerberus',
			'🐉hydra',
			'🐍medusa',
			'🦅harpy',
			'🦁sphinx',
			'🐺werewolf',
			'🐉loch ness monster',
			'🦶bigfoot',
			'🦶yeti',
			'🐐chupacabra',
			'🐇jackalope',
			'🏜️desert',
			'🏝️oasis',
			'🐪camel',
			'🏜️caravan',
			'🍎apple',
			'🍌banana',
			'🍒cherry',
			'🍉watermelon',
			'🍓strawberry',
			'🐋whale',
			'🐬dolphin',
			'🍇grape',
			'🍦ice cream',
			'🏠going home',
			'🏫going to school',
			'🍫chocolate',
			'🍕pizza',
			'🍔burger',
			'🌭hot dog',
			'🥪sandwich',
			'🍪cookie',
			'🍰cake',
			'🐧penguin',
			'🐯tiger',
			'🦁lion',
			'🐘elephant',
			'🐒monkey',
			'🐻bear',
			'🐶dog',
			'🐱cat',
			'🐦bird',
			'🐴horse',
			'🐑sheep',
			'🐄cow',
			'🐷pig',
			'🐔chicken',
			'🦆duck',
			'🦃turkey',
			'🐐goat',
			'🐰rabbit',
			'🐺wolf',
			'🐢turtle',
			'🐍snake',
			'🐟fish',
			'🐙octopus',
			'🦀crab',
			'🦈shark',
			'🐴seahorse',
			'🐟starfish',
			// Add emojis for NOUNS
			'🍎apple',
			'👶baby',
			'🔙back',
			'⚽ball',
			'🐻bear',
			'🛏️bed',
			'🔔bell',
			'🐦bird',
			'🎂birthday',
			'⛵boat',
			'📦box',
			'👦boy',
			'🍞bread',
			'👨‍👦brother',
			'🎂cake',
			'🚗car',
			'🐱cat',
			'🪑chair',
			'🐔chicken',
			'👶children',
			'🎄Christmas',
			'🧥coat',
			'🌽corn',
			'🐄cow',
			'📅day',
			'🐕dog',
			'🪆doll',
			'🚪door',
			'🦆duck',
			'🥚egg',
			'👁️eye',
			'🌾farm',
			'👨‍🌾farmer',
			'👨father',
			'🦶feet',
			'🔥fire',
			'🐟fish',
			'🪟floor',
			'🌸flower',
			'🎮game',
			'🌻garden',
			'👧girl',
			'👋goodbye',
			'🌿grass',
			'🌍ground',
			'✋hand',
			'👤head',
			'⛰️hill',
			'🏠home',
			'🐴horse',
			'🏠house',
			'🐱kitty',
			'🦵leg',
			'✉️letter',
			'👨man',
			'👨‍👨‍👦men',
			'🥛milk',
			'💰money',
			'🌅morning',
			'👩mother',
			'👤name',
			'🪺nest',
			'🌃night',
			'📄paper',
			'🎉party',
			'🖼️picture',
			'🐷pig',
			'🐰rabbit',
			'🌧️rain',
			'💍ring',
			'🦜robin',
			'🎅Santa Claus',
			'🏫school',
			'🌱seed',
			'🐑sheep',
			'👞shoe',
			'👧sister',
			'❄️snow',
			'🎵song',
			'🐿️squirrel',
			'🪵stick',
			'🛣️street',
			'☀️sun',
			'🪑table',
			'🔧thing',
			'⏰time',
			'🔝top',
			'🧸toy',
			'🌳tree',
			'⌚watch',
			'💧water',
			'🛣️way',
			'🌬️wind',
			'🪟window',
			'🪵wood'
		])
	);

	function handleClick(index) {
		if (winner || board[index]) return;
		board[index] = currentPlayer;
		checkWinner();
		currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
	}

	function checkWinner() {
		const combinations = [
			[0, 1, 2],
			[3, 4, 5],
			[6, 7, 8],
			[0, 3, 6],
			[1, 4, 7],
			[2, 5, 8],
			[0, 4, 8],
			[2, 4, 6]
		];

		for (const combination of combinations) {
			if (
				board[combination[0]] &&
				board[combination[0]] === board[combination[1]] &&
				board[combination[0]] === board[combination[2]]
			) {
				winner = board[combination[0]];
				winningCombination = combination;
				return;
			}
		}

		if (!board.includes(null)) {
			winner = 'draw';
		}
	}

	function handleRestart() {
		board = Array(9).fill(null);
		currentPlayer = 'X';
		winner = null;
		winningCombination = null;
	}
</script>

<div class="board">
	{#each board as cell, index}
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div
			class={`cell ${cell === 'X' ? 'x' : cell === 'O' ? 'o' : ''} ${winningCombination && winningCombination.includes(index) ? 'winning-cell' : ''}`}
			on:click={() => handleClick(index)}
		>
			{cell
				? cell === 'X'
					? words[Math.floor(Math.random() * words.length)]
					: words[Math.floor(Math.random() * words.length)]
				: ''}
		</div>
	{/each}
</div>

{#if winner}
	<div class="result">
		{#if winner === 'draw'}
			It's a draw!
		{:else}
			Player {winner} wins!
		{/if}
		<button on:click={handleRestart}>Restart</button>
	</div>
{:else}
	<div class="turn">
		Player {currentPlayer}'s turn
	</div>
{/if}

<style>
	.board {
		display: grid;
		width: 460px;
		margin: 0 auto;
		grid-template-columns: repeat(3, 1fr);
		grid-gap: 10px;
		border-radius: 1rem;
		background-color: rgb(193, 193, 193);
	}

	.cell {
		border-radius: 1rem;
		/* width: 150px; */
		height: 150px;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 24px;
		font-weight: bold;
		text-align: center;
		cursor: pointer;
		box-shadow:
			rgba(50, 50, 93, 0.25) 0px 6px 12px -2px,
			rgba(0, 0, 0, 0.3) 0px 3px 7px -3px;
	}

	.cell:hover {
		box-shadow:
			rgb(85, 91, 255) 0px 0px 0px 3px,
			rgb(31, 193, 27) 0px 0px 0px 6px,
			rgb(255, 217, 19) 0px 0px 0px 9px,
			rgb(255, 156, 85) 0px 0px 0px 12px;
	}

	.cell:active {
		box-shadow:
			rgba(0, 0, 0, 0.25) 0px 54px 55px,
			rgba(0, 0, 0, 0.12) 0px -12px 30px,
			rgba(0, 0, 0, 0.12) 0px 4px 6px,
			rgba(0, 0, 0, 0.17) 0px 12px 13px,
			rgba(0, 0, 0, 0.09) 0px -3px 5px;
	}
	.x {
		background-color: #2f6eff;
	}

	.o {
		background-color: #ffca2a;
	}

	.winning-cell {
		/* background-color: #8cf652; */

		font-weight: bold;
		animation: mymove 2s infinite;
	}

	.winning-cell::after {
		content: '👍';
		font-size: 4rem;
		position: absolute;
		top: -10px;
		right: -10px;
		animation: winEmoji 1.5s infinite;
	}

	@keyframes winEmoji {
		0% {
			transform: rotate(0);
			top: -10px;
		}
		30% {
			transform: rotate(-20deg);
			top: -70px;
		}
		100% {
			transform: rotate(1deg);
			translate-y: -10px;
		}
	}

	@keyframes mymove {
		50% {
			transform: rotate(2deg);
			translate-y: 4px;
			background-color: #6ace34;
			margin: 1px;
		}
		100% {
			transform: rotate(-2deg);
			translate-y: -4px;
		}
	}

	.result {
		margin: 10px auto;
		font-size: 24px;
	}

	.turn {
		margin-top: 20px;
		font-size: 24px;
		margin: 10px auto;
	}
</style>
