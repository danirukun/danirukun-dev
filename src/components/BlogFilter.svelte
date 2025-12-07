<script lang="ts">
	import { onMount } from 'svelte';

	export let posts: any[];
	export let categories: string[];

	let selectedCategory: string | null = null;

	$: filteredPosts = selectedCategory === null
		? posts
		: posts.filter((post) =>
				post.categories?.some((cat: { title: string }) => cat.title === selectedCategory)
			);

	function selectCategory(category: string | null) {
		selectedCategory = selectedCategory === category ? null : category;
	}

	onMount(() => {
		const urlParams = new URLSearchParams(window.location.search);
		const categoryParam = urlParams.get('category');
		if (categoryParam && categories.includes(categoryParam)) {
			selectedCategory = categoryParam;
		}
	});
</script>

<div class="mb-8 flex flex-wrap items-center justify-center gap-3">
	<button
		onclick={() => selectCategory(null)}
		class="rounded-full border border-border px-4 py-2 text-sm font-semibold transition-colors {selectedCategory === null
			? 'bg-primary text-primary-foreground'
			: 'bg-secondary text-secondary-foreground hover:bg-secondary/80'}"
	>
		All
	</button>
	{#each categories as category}
		<button
			onclick={() => selectCategory(category)}
			class="rounded-full border border-border px-4 py-2 text-sm font-semibold transition-colors {selectedCategory === category
				? 'bg-primary text-primary-foreground'
				: 'bg-secondary text-secondary-foreground hover:bg-secondary/80'}"
		>
			{category}
		</button>
	{/each}
</div>

<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
	{#each filteredPosts as post}
		<article class="group">
			<a href={`/blog/${post.slug.current}/`} class="no-underline block h-full">
				<div class="h-full flex flex-col overflow-hidden rounded-lg border border-border bg-card shadow-sm transition-all duration-300 hover:shadow-lg hover:border-primary/20 hover:-translate-y-1">
					<div class="relative aspect-video w-full overflow-hidden bg-muted">
						<img
							class="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
							src={post.imageUrl}
							alt={post.imageAlt}
							loading="lazy"
						/>
						<div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent opacity-0 transition-opacity duration-300 group-hover:opacity-100"></div>
					</div>
					<div class="flex flex-1 flex-col p-6">
						<h3 class="mb-2 text-xl font-semibold tracking-tight text-card-foreground transition-colors group-hover:text-primary line-clamp-2">
							{post.title}
						</h3>
						<div class="mt-auto flex items-center justify-between">
							<p class="text-sm text-muted-foreground">
								{new Date(post._updatedAt).toLocaleDateString('en-us', {
									year: 'numeric',
									month: 'short',
									day: 'numeric',
								})}
							</p>
							{#if post.categories && post.categories.length > 0}
								<div class="flex flex-wrap gap-1">
									{#each post.categories.slice(0, 2) as cat}
										<span class="rounded-full bg-muted px-2 py-0.5 text-xs text-muted-foreground">
											{cat.title}
										</span>
									{/each}
								</div>
							{/if}
						</div>
					</div>
				</div>
			</a>
		</article>
	{/each}
</div>

{#if filteredPosts.length === 0}
	<div class="col-span-full py-12 text-center">
		<p class="text-muted-foreground">No posts found in this category.</p>
	</div>
{/if}

