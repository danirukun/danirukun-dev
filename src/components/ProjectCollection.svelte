<script lang="ts">
	import ProjectEntry from './ProjectEntry.svelte';
	import type { Project } from '../types';

	export let projects: Project[];
</script>

<section class="grid grid-cols-1 gap-6 px-4 py-8 sm:grid-cols-2 lg:grid-cols-3 lg:px-8">
	{#each projects as project}
		<ProjectEntry {...project} />
	{/each}
</section>
