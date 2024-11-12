<script>
	import { page } from '$app/stores';
	import logo from '$lib/images/svelte-logo.svg';
	import github from '$lib/images/github.svg';
</script>

<header>
	<div class="corner">
		<a href="https://kit.svelte.dev">
			<img src={logo} alt="SvelteKit" />
		</a>
	</div>

	<nav>
		<svg viewBox="0 0 2 3" aria-hidden="true">
			<path d="M0,0 L1,2 C1.5,3 1.5,3 2,3 L2,0 Z" />
		</svg>
		<ul>
			<li aria-current={$page.url.pathname === '/' ? 'page' : undefined}>
				<a href="/">Home</a>
			</li>
			<li aria-current={$page.url.pathname === '/dnd' ? 'page' : undefined}>
				<a href="/dnd">dnd</a>
			</li>
			<li aria-current={$page.url.pathname === '/tictactoe' ? 'page' : undefined}>
				<a href="/tictactoe">TicTacToe</a>
			</li>
			<li aria-current={$page.url.pathname === '/quiz' ? 'page' : undefined}>
				<a href="/quiz">quiz</a>
			</li>
			<li aria-current={$page.url.pathname === '/draw' ? 'page' : undefined}>
				<a href="/draw">draw</a>
			</li>
			<li aria-current={$page.url.pathname === '/run' ? 'page' : undefined}>
				<a href="/run">run</a>
			</li>
			<li aria-current={$page.url.pathname === '/sightwordstyping' ? 'page' : undefined}>
				<a href="/sightwordstyping">swt</a>
			</li>

			<li aria-current={$page.url.pathname.startsWith('/sverdle') ? 'page' : undefined}>
				<a href="/sverdle">Sverdle</a>
			</li>
			<li aria-current={$page.url.pathname === '/dinojump' ? 'page' : undefined}>
				<a href="/dinojump">dino</a>
			</li>
			<li aria-current={$page.url.pathname === '/swt2' ? 'page' : undefined}>
				<a href="/swt2">swt2</a>
			</li>
			<li aria-current={$page.url.pathname === '/flashcards' ? 'page' : undefined}>
				<a href="/flashcards">fc</a>
			</li>
			<li aria-current={$page.url.pathname === '/connect4' ? 'page' : undefined}>
				<a href="/connect4">cnt4</a>
			</li>
			<li aria-current={$page.url.pathname === '/connect3' ? 'page' : undefined}>
				<a href="/connect3">cnt3</a>
			</li>
			<li aria-current={$page.url.pathname === '/flashcardsw' ? 'page' : undefined}>
				<a href="/flashcardsw">fcsw</a>
			</li>
			<li aria-current={$page.url.pathname === '/whack' ? 'page' : undefined}>
				<a href="/whack">whack</a>
			</li>
		</ul>
		<svg viewBox="0 0 2 3" aria-hidden="true">
			<path d="M0,0 L0,3 C0.5,3 0.5,3 1,2 L2,0 Z" />
		</svg>
	</nav>

	<div class="corner">
		<a href="https://github.com/sveltejs/kit">
			<img src={github} alt="GitHub" />
		</a>
	</div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
	}

	.corner {
		width: 3em;
		height: 3em;
	}

	.corner a {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.corner img {
		width: 2em;
		height: 2em;
		object-fit: contain;
	}

	nav {
		display: flex;
		justify-content: center;
		--background: rgba(255, 255, 255, 0.7);
	}

	svg {
		width: 2em;
		height: 3em;
		display: block;
	}

	path {
		fill: var(--background);
	}

	ul {
		position: relative;
		padding: 0;
		margin: 0;
		height: 3em;
		display: flex;
		justify-content: center;
		align-items: center;
		list-style: none;
		background: var(--background);
		background-size: contain;
		flex-wrap: wrap; /* Enable wrapping of li elements */
	}

	li {
		position: relative;
		height: 100%;
	}

	li[aria-current='page']::before {
		--size: 6px;
		content: '';
		width: 0;
		height: 0;
		position: absolute;
		top: 0;
		left: calc(50% - var(--size));
		border: var(--size) solid transparent;
		border-top: var(--size) solid var(--color-theme-1);
	}

	nav a {
		display: flex;
		height: 100%;
		align-items: center;
		padding: 0 0.5rem;
		color: var(--color-text);
		font-weight: 700;
		font-size: 0.8rem;
		text-transform: uppercase;
		letter-spacing: 0.1em;
		text-decoration: none;
		transition: color 0.2s linear;
	}

	a:hover {
		color: var(--color-theme-1);
	}
</style>
