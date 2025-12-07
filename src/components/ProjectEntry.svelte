<script lang="ts">
	import { urlForImage } from '../sanity/lib/urlForImage';
	import type { SanityImage } from '../types';
	import placeholderImg from '../images/blog-index-placeholder.png';

	export let name: string;
	export let description: string;
	export let projectURL: string | undefined = undefined;
	export let thumbnail: SanityImage | undefined = undefined;

	$: thumbnailUrl = thumbnail ? urlForImage(thumbnail.asset).url() : placeholderImg.src;
	$: thumbnailAlt = thumbnail?.alt || name;
</script>

{#if projectURL !== undefined}
	<a href={projectURL} class="no-underline group block h-full" aria-label={projectURL}>
		<article
			class="h-full flex flex-col overflow-hidden rounded-lg border border-border bg-card shadow-sm transition-all duration-300 hover:shadow-lg hover:border-primary/20 hover:-translate-y-1"
		>
			<div class="relative aspect-video w-full overflow-hidden bg-muted">
				<img
					class="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
					src={thumbnailUrl}
					alt={thumbnailAlt}
					loading="lazy"
				/>
				<div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent opacity-0 transition-opacity duration-300 group-hover:opacity-100"></div>
			</div>
			<div class="flex flex-1 flex-col p-6">
				<h2 class="mb-3 text-2xl font-semibold tracking-tight text-card-foreground transition-colors group-hover:text-primary">
					{name}
				</h2>
				<p class="flex-1 text-sm leading-relaxed text-muted-foreground">
					{description}
				</p>
			</div>
		</article>
	</a>
{:else}
	<article
		class="h-full flex flex-col overflow-hidden rounded-lg border border-border bg-card shadow-sm"
	>
		<div class="relative aspect-video w-full overflow-hidden bg-muted">
			<img
				class="h-full w-full object-cover"
				src={thumbnailUrl}
				alt={thumbnailAlt}
				loading="lazy"
			/>
		</div>
		<div class="flex flex-1 flex-col p-6">
			<h2 class="mb-3 text-2xl font-semibold tracking-tight text-card-foreground">
				{name}
			</h2>
			<p class="flex-1 text-sm leading-relaxed text-muted-foreground">
				{description}
			</p>
		</div>
	</article>
{/if}
