<script lang="ts">
	export let icon: any;
	export let href: string;
	export let title: string;
	export let body: string;
</script>

<a
	{href}
	class="flex h-44 w-44 flex-col items-center justify-evenly gap-4 rounded-xl bg-muted/40 px-3 py-5 text-center shadow-md hover:scale-105 md:h-64 md:w-64 md:justify-normal md:py-10"
>
	<svelte:component this={icon} class="hidden md:block md:h-20 md:w-20" />
	<h1 class="text-md font-bold md:text-lg">{title}</h1>
	<p class="text-xs md:text-sm">{body}</p>
</a>
