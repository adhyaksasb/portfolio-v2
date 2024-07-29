<script lang="ts">
	import { keyboardTopKeys, keyboardMiddleKeys, keyboardBottomKeys } from '$lib/data/playground';
	import { CornerDownLeft, Delete } from 'lucide-svelte';

	let {
		onKeyClick,
		keyColors
	}: { onKeyClick: (key: string) => void; keyColors: { [key: string]: string } } = $props();

	console.log(keyColors);
</script>

<div class="grid w-screen grid-cols-10 gap-1 md:w-[32rem] md:gap-1.5">
	{#each keyboardTopKeys as key}
		<button
			type="button"
			class={`rounded-md border p-2 text-xs md:text-lg ${keyColors[key] || 'bg-muted'}`}
			onclick={() => onKeyClick(key)}>{key}</button
		>
	{/each}
</div>
<div class="grid w-screen grid-cols-9 gap-1 md:w-[32rem] md:gap-1.5">
	{#each keyboardMiddleKeys as key}
		<button
			type="button"
			class={`rounded-md border p-2 text-xs md:text-lg ${keyColors[key] || 'bg-muted'}`}
			onclick={() => onKeyClick(key)}>{key}</button
		>
	{/each}
</div>
<div class="grid w-screen grid-cols-9 gap-1 md:w-[32rem] md:grid-cols-11 md:gap-1.5">
	<button
		type="button"
		class="col-span-1 flex items-center justify-center rounded-md border bg-muted p-2 text-xs md:col-span-2 md:text-lg"
		onclick={() => onKeyClick('Backspace')}
	>
		<Delete class="h-4 w-4 md:h-7 md:w-7" />
	</button>
	{#each keyboardBottomKeys as key}
		<button
			type="button"
			class={`rounded-md border p-2 text-xs md:text-lg ${keyColors[key] || 'bg-muted'}`}
			onclick={() => onKeyClick(key)}>{key}</button
		>
	{/each}
	<button
		type="button"
		class="col-span-1 flex items-center justify-center rounded-md border bg-muted p-2 text-xs md:col-span-2 md:text-lg"
		onclick={() => onKeyClick('Enter')}
	>
		<CornerDownLeft class="h-4 w-4 md:h-7 md:w-7" />
	</button>
</div>
