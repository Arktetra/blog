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
			if (heading.tagName === 'H3') {
				div.style.paddingLeft = '2em';
			}
			div.addEventListener('click', function () {
				heading.scrollIntoView({ behavior: 'smooth' });
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
	/* article { */
	/* 	padding-inline: calc(8px + 1.5625vw); */
	/* } */

	.metadata-container, .content {
		padding-inline: calc(8px + 1.5625vw);
	}

	.metadata-container {
		padding-block: calc(8px + 1.5626vw);
		/* background: repeating-linear-gradient( */
		/* 	-45deg, */
		/* 	var(--background-color), */
		/* 	var(--background-color) 1px, */
		/* 	var(--theme-color-shade) 1px, */
		/* 	var(--theme-color-shade) 2px */
		/* ); */

		background: repeating-linear-gradient(
			-45deg,
			var(--background),
			var(--background) 1px,
			var(--theme-color-shade) 1px,
			var(--theme-color-shade) 2px
		);
		border-bottom: 0.1rem solid var(--border);
	}

	.metadata {
		padding-block: calc(8px + 1.5625vh);
	}

	h1 {
		font-size: clamp(2rem, 2.9vw, 2.9rem);
		color: var(--theme-color-base);
		line-height: 1.2;
	}

	hgroup p {
		font-size: calc(12px + 0.390625vw);
		color: var(--text-2);
		padding-block: calc(2px + 0.56vw);
	}

	.tags > * {
		/* background-color: var(--theme-color-shade); */
		background-color: var(--tag-color);
		border-radius: 2rem;
		padding-inline: calc(2px + 0.56vw);
		padding-block: 0.2rem;
	}

	.content {
		display: flex;
		gap: 2rem;
	}

	.prose {
		padding-block-start: 1rem;
	}

	#toc {
		font-size: calc(12px + 0.390625vw);
		width: 100%;
		position: sticky;
		position: -webkit-sticky;
		display: none;
		top: 0;
		align-self: flex-start;
	}

	@media screen and (min-width: 90ch) {
		#toc {
			display: block;
		}

		.metadata, .content {
			margin-inline: auto;
			max-inline-size: var(--content-size);
		}

		#toc {
			max-inline-size: 20ch;
		}
	}
</style>
