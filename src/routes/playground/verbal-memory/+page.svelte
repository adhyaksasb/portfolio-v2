<script lang="ts">
	import { getRandomIndex } from '$lib/utils';
	import { verbalWords } from '$lib/data/playground';

	import { BookA } from 'lucide-svelte';

	let words: string[] = verbalWords;
	let memoryWords: (string | null)[] = [];
	let lives: number = $state(3);
	let scores: number = $state(0);
	let isGameRun: boolean = $state(false);
	let isGameOver: boolean = $state(false);

	let word = $state(words[getRandomIndex<string>(words)]);

	const startGame = () => {
		lives = 3;
		scores = 0;
		isGameRun = true;
		isGameOver = false;
	};

	const seenButton = (val: string) => {
		console.log(memoryWords);

		if (!memoryWords.includes(val)) {
			lives -= 1;
			if (lives <= 0) {
				isGameOver = true;
				isGameRun = false;
			}
			memoryWords.push(val);
			word = words[getRandomIndex<string>(words)];
		} else {
			scores += 1;
			word = words[getRandomIndex<string>(words)];
		}
	};

	const newButton = (val: string) => {
		console.log(memoryWords);
		if (!memoryWords.includes(val)) {
			scores += 1;
			memoryWords.push(val);
			word = words[getRandomIndex<string>(words)];
		} else {
			lives -= 1;
			if (lives <= 0) {
				isGameOver = true;
				isGameRun = false;
			}
			word = words[getRandomIndex<string>(words)];
		}
	};
</script>

{#if isGameRun}
	<div class="flex w-full flex-col items-center justify-center gap-10 px-4 md:w-96">
		<div class="flex w-full justify-between">
			<div class="flex gap-4">
				<p class="text-xl text-muted-foreground">Lives |</p>
				<p class="text-xl text-foreground">{lives}</p>
			</div>
			<div class="flex gap-4">
				<p class="text-xl text-muted-foreground">Scores |</p>
				<p class="text-xl text-foreground">{scores}</p>
			</div>
		</div>
		<div class="mb-6 flex items-center justify-center">
			<h1 class="text-4xl font-bold md:text-6xl">{word}</h1>
		</div>
		<div class="flex gap-14">
			<button
				type="button"
				class="rounded-lg bg-blue-500 px-5 py-4 text-lg text-white"
				onclick={() => seenButton(word)}
			>
				Seen
			</button>
			<button
				type="button"
				class="rounded-lg bg-blue-500 px-5 py-4 text-lg text-white"
				onclick={() => newButton(word)}
			>
				New
			</button>
		</div>
	</div>
{:else if isGameOver}
	<BookA class="mb-6 h-44 w-44" />
	<h1 class="mb-3 text-4xl font-bold">Verbal Memory</h1>
	<p class="text-lg">
		You've remembered <span class="text-2xl font-bold">{memoryWords.length} words</span> and you've
		got <span class="text-2xl font-bold">{scores} scores</span>
	</p>
	<button class="mt-6 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
		>Play Again</button
	>
{:else}
	<BookA class="mb-6 h-40 w-40" />
	<h1 class="mb-3 text-4xl font-bold">Verbal Memory</h1>
	<p class="text-lg">Keep as many words in short term memory as possible</p>
	<button class="mt-6 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
		>Play Game</button
	>
{/if}
