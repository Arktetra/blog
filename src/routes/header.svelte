<script lang="ts">
	import * as config from '$lib/config';
	import { theme } from '$lib/theme.svelte';
	import Toggle from './toggle.svelte';

	let isActive = false;

	function toggleOffScreen() {
		isActive = !isActive;
	}
</script>

<nav>
	<div class="menu">
		<a href="https://arktetra.github.io" class="title">
			<b>{config.title}</b>
		</a>

		<ul class="links">
			<li><a href="https://arktetra.github.io/blog">Blog</a></li>
			<li>Reading List</li>
			<li><Toggle /></li>
		</ul>
		<div
			class="ham-menu"
			class:active={isActive}
			role="button"
			aria-label="toggle menu"
			tabindex="0"
			on:click={toggleOffScreen}
			on:keydown={() => {}}
		>
			<span></span>
			<span></span>
			<span></span>
		</div>
	</div>
	<div class="offscreen-menu" class:active={isActive}>
		<div><a href="https://arktetra.github.io/blog">Blog</a></div>
		<div>Reading List</div>
		<button on:click={theme.toggle} aria-label="Toggle theme">
			{#if theme.current === 'dark'}
				Light Mode
			{:else}
				Dark Mode
			{/if}
		</button>
	</div>
	<div id="overlay" class:active={isActive}></div>
</nav>

<style>
	nav {
		border-bottom: 1px solid var(--theme-color-shade);
	}

	a {
		position: relative;
		text-decoration: none;
		color: inherit;
	}

	a::before {
		content: '';
		position: absolute;
		display: block;
		width: 100%;
		height: 2px;
		bottom: 0;
		left: 0;
		background-color: var(--theme-color-base);
		transform: scaleX(0);
		transition: transform 0.3s ease;
	}

	a:hover::before {
		transform: scaleX(1);
	}

	button {
		padding: 0;
		font-weight: inherit;
		background: none;
		border: none;
		box-shadow: none;
		overflow: hidden;
	}

	.title {
		font-size: 2rem;
		color: var(--theme-color-base);
	}

	.menu {
		display: flex;
		justify-content: space-between;
		margin-inline: auto;
		padding: calc(8px + 1.5625vw);
		align-items: center;
	}

	ul {
		list-style: none;
	}

	.links {
		display: flex;
		gap: 1.5em;
		color: var(--text-1);
	}

	.ham-menu {
		display: none;
		height: 50px;
		width: 40px;
		margin-left: auto;
		position: relative;
		align-self: center;
	}

	.ham-menu span {
		height: 5px;
		width: 100%;
		background-color: var(--theme-color-base);
		border-radius: 25px;
		position: absolute;
		left: 50%;
		top: 50%;
		transform: translate(-50%, -50%);
		transition: 0.3s ease;
		z-index: 3;
	}

	.ham-menu span:nth-child(1) {
		top: 25%;
	}

	.ham-menu span:nth-child(3) {
		top: 75%;
	}

	.ham-menu.active span:nth-child(1) {
		top: 50%;
		transform: translate(-50%, -50%) rotate(45deg);
	}

	.ham-menu.active span:nth-child(2) {
		opacity: 0;
	}

	.ham-menu.active span:nth-child(3) {
		top: 50%;
		transform: translate(-50%, -50%) rotate(-45deg);
	}

	.offscreen-menu {
		position: fixed;
		height: 50%;
		width: 100%;
		max-width: 450px;
		top: 0;
		right: -450px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text-align: center;
		transition: 0.3s ease;
		background-color: var(--background);
		gap: 1em;
		color: var(--text-1);
		font-size: 1.5rem;
		border-bottom: 0.1rem solid var(--border);
		border-left: 0.1rem solid var(--border);
		overflow: hidden;
		z-index: 2;
	}

	.offscreen-menu > * {
		border-bottom: 0.1rem solid var(--border);
	}

	.offscreen-menu a {
		color: var(--text-1);
	}

	.offscreen-menu.active {
		right: 0;
	}

	#overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0);
		z-index: 1;
	}

	#overlay.active {
		background-color: rgba(0, 0, 0, 0.6);
	}

	@media screen and (min-width: 90ch) {
		.menu {
			max-width: var(--content-size);
		}
	}

	@media screen and (max-width: 628px) {
		.links {
			display: none;
		}

		.ham-menu {
			display: block;
		}

		:global(body):has(.offscreen-menu.active) {
			overflow: hidden !important;
		}
	}

	@media screen and (min-width: 628px) {
		.offscreen-menu {
			display: none;
		}
	}
</style>
