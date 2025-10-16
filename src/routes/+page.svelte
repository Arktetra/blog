<script lang="ts">
	import { formatDate } from '$lib/utils';
	import * as config from '$lib/config';

	let data = $props();
</script>

<svelte:head>
	<title>{config.title}</title>
</svelte:head>

<section>
	<ul class="posts">
		{#each data.data.posts as post}
			<li class="post">
				<h1><a href={post.slug} class="title">{post.title}</a></h1>
				<p class="date">{formatDate(post.date)}</p>
				<p class="description">{post.description}</p>
			</li>
		{/each}
	</ul>
</section>

<style>
	.posts {
		display: grid;
		justify-items: center;
		padding-inline: calc(8px + 1.5625vw);
	}

	.post {
		max-inline-size: var(--content-size);
		padding-block: 1.5em;
		border-bottom: 0.1em solid var(--border);
		width: 100%;

		h1 a {
			color: inherit;
			text-decoration: none;
		}
	}

	.title {
		text-transform: capitalize;
	}

	.date {
		margin-block: 0.5em;
	}

	.date,
	.description {
		color: var(--text-2);
	}

	li {
		list-style-type: none;
	}

	h1 {
		font-size: 2.96rem;
	}

	@media screen and (max-width: 628px) {
		h1 {
			font-size: 2rem;
		}
	}
</style>
