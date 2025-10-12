<script lang="ts">
	import * as config from '$lib/config';
	import { page } from '$app/state';
	import { formatDate } from '$lib/utils';
	import type { Attachment } from 'svelte/attachments';

	const h2Extractor: Attachment = (element) => {
		const h2Elements = document.querySelectorAll('h2, h3');

		h2Elements.forEach((heading) => {
			const div = document.createElement('div');
			div.textContent = heading.textContent;
			if (heading.tagName === "H3") {
				div.style.textIndent = "1em";
			}
			div.addEventListener("click", function() {
				heading.scrollIntoView({ behavior: "smooth" });
			});
			element.appendChild(div);
		});
	};
</script>

<svelte:head>
	<title>{page.data.meta.title}</title>
	<meta property="og:type" content="article" />
	<meta property="og:title" content={page.data.meta.title} />
</svelte:head>

<article>
		<div class="metadata-container">
			<div class="metadata">
				<hgroup>
					<h1>{page.data.meta.title}</h1>
					<p>Published at {formatDate(page.data.meta.date)}</p>
				</hgroup>

				<div class="tags">
					{#each page.data.meta.categories as category}
						<span class="surface-4">&num;{category}</span>
					{/each}
				</div>
			</div>
		</div>

		<div class="content">
			<div class="prose">
				<svelte:component this={page.data.content} />
			</div>
			<div id="toc" {@attach h2Extractor}>
				<p>Contents</p>
			</div>
		</div>
</article>

<style>
	.metadata-container {
		width: 100%;
		padding-block: var(--size-4);
		background: repeating-linear-gradient(
			-45deg,
			var(--background),
			var(--background) 1px,
			var(--surface-1) 1px,
			var(--surface-1) 2px
		);

		border-bottom: 1px solid var(--border);
	}

	.metadata {
		max-inline-size: 1100px;
		margin-inline: auto;
		padding-inline: var(--size-7);
	}

	.content {
		max-width: 1100px;
		margin-inline: auto;
		padding-inline: var(--size-7);
		display: flex;
		gap: 3em;
	}

	.prose {
		max-width: 850px;
		min-width: 800px;
	}

	hgroup {
		width: 100%;
	}


	#toc {
		width: 100%;
		position: sticky;
		position: -webkit-sticky;
		top: 0;
		align-self: flex-start;
	}

	hgroup {
		width: 950px;
	}

	h1 {
		text-transform: capitalize;
		font-weight: normal;
	}

	h1 + p {
		/* width: 100%; */
		margin-top: var(--size-2);
		color: var(--text-2);
	}

	.tags {
		/* width: 100%; */
		display: flex;
		gap: var(--size-3);
		margin-top: var(--size-3);
	}

	.tags > * {
		padding: var(--size-2) var(--size-3);
		border-radius: var(--radius-round);
	}
</style>
