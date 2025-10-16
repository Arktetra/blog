<script lang="ts">
	import * as config from '$lib/config';
	import Toggle from './toggle.svelte';
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
	</div>
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

	@media screen and (min-width: 90ch) {
		.menu {
			max-width: var(--content-size);
		}
	}
</style>
