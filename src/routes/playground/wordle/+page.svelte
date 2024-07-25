<script lang="ts">
	import { keyboardTopKeys, keyboardMiddleKeys } from '$lib/data/playground';

	import { Box, Delete } from 'lucide-svelte';
	let isGameRun: boolean = $state(true);

	let board: (string | null)[] = $state(Array(30).fill(null));

	const startGame = () => {
		isGameRun = true;
	};
</script>

{#if isGameRun}
	<div class="mb-4 grid grid-cols-5 gap-2">
		{#each board as cell, i}
			<div class="rounded-md border bg-muted p-3 text-center">01</div>
		{/each}
	</div>
	<div class="grid w-[36rem] grid-cols-10 gap-1.5">
		{#each keyboardTopKeys as key}
			<div class="rounded-md border bg-muted p-3 text-center">{key}</div>
		{/each}
	</div>
	<div class="grid w-[36rem] grid-cols-9 gap-1.5">
		{#each keyboardMiddleKeys as key}
			<div class="rounded-md border bg-muted p-3 text-center">{key}</div>
		{/each}
	</div>
	<div class="grid w-[36rem] grid-cols-11 gap-1.5">
		<div class="col-span-2 flex items-center justify-center rounded-md border bg-muted p-3">
			<Delete class="h-7 w-7" />
		</div>
	</div>
{:else}
	<Box class="mb-6 h-40 w-40" />
	<h1 class="mb-3 text-4xl font-bold">Wordle</h1>
	<p class="text-lg">Try guess the hidden word</p>
	<button class="mt-6 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
		>Play Game</button
	>
{/if}
