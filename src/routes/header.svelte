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
			<li><a href="https://arktetra.github.io/neural-flux">Blog</a></li>
			<li>Reading List</li>
			<li><Toggle /></li>
		</ul>
	</div>
</nav>

<style>
	a {
		color: inherit;
		text-decoration: none;
	}

	nav {
		display: flex;
		justify-content: center;
		/* justify-content: space-between; */
		padding-block: var(--size-7);
		padding-inline: var(--size-7);
		margin-left: 0;
		border-bottom: 1px solid var(--border);
		/* align-items: center; */
	}

	nav ul {
		list-style: none;
		align-items: center;
		color: var(--text-1);
	}

	.menu {
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 100%;
		/* max-inline-size: var(--size-content-7); */
		max-inline-size: 1096px;
		align-items: center;
	}

	.links {
		display: flex;
		gap: var(--size-7);
		margin-block: 0;
	}

	.title {
		font-size: 24px;
	}
</style>
