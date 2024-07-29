<script lang="ts">
	import VirtualKeyboard from '$lib/components/Playground/VirtualKeyboard.svelte';
	import { WORDS, getRandomWord } from '$lib/wordle';
	import { onMount, onDestroy, tick } from 'svelte';
	import { Box } from 'lucide-svelte';
	let isGameRun: boolean = $state(false);
	let isGameOver: boolean = $state(false);
	let isGameWon: boolean = $state(false);
	let showMessage: boolean = $state(false);
	let message: string = $state('');
	let isMobile: boolean = $state(false);

	const hideMessage = () => {
		showMessage = false;
	};

	let targetWord = $state(getRandomWord().toLowerCase());
	let wordLength = 5;
	const maxAttempts = 6;

	let attempts: string[][] = $state(
		Array(maxAttempts)
			.fill(null)
			.map(() => Array(wordLength).fill(''))
	);
	let currentAttempt = $state(0);
	let currentInputIndex = $state(0);
	let inputRow: string[] = Array(wordLength).fill('');
	let colors: string[][] = $state(
		Array(maxAttempts)
			.fill(null)
			.map(() => Array(wordLength).fill('bg-muted'))
	); // Colors for each row
	let mergedInput: string = '';

	const startGame = () => {
		isGameRun = true;
		isGameOver = false;
		isGameWon = false;
		targetWord = getRandomWord().toLowerCase();
		attempts = Array(maxAttempts)
			.fill(null)
			.map(() => Array(wordLength).fill(''));
		currentAttempt = 0;
		currentInputIndex = 0;
		inputRow = Array(wordLength).fill('');
		colors = Array(maxAttempts)
			.fill(null)
			.map(() => Array(wordLength).fill('bg-muted'));
		mergedInput = '';
		tick().then(() => {
			(
				document.getElementById(
					`word-input-${currentAttempt}-${currentInputIndex}`
				) as HTMLInputElement
			).focus();
		});
	};

	const handleInput = (index: number, event: Event) => {
		const inputElement = event.target as HTMLInputElement;
		const value = inputElement.value;

		// Ensure only a single character is allowed
		if (value.length > 1) {
			inputElement.value = value.charAt(0);
		}

		inputRow = [
			...inputRow.slice(0, index),
			inputElement.value.charAt(0),
			...inputRow.slice(index + 1)
		];

		// Automatically focus on the next input if current input has a value
		if (inputElement.value.length === 1 && index < wordLength - 1) {
			currentInputIndex = index + 1;
			(
				document.getElementById(`word-input-${currentAttempt}-${index + 1}`) as HTMLInputElement
			).focus();
		}
	};

	const handleKeyDown = (index: number, event: KeyboardEvent) => {
		if (isGameOver || isMobile) {
			event.preventDefault();
			return;
		}
		if (event.key === 'Backspace' && inputRow[index] === '' && index > 0) {
			currentInputIndex = index - 1;
			(
				document.getElementById(`word-input-${currentAttempt}-${index - 1}`) as HTMLInputElement
			).focus();
		} else if (event.key === 'Enter') {
			mergedInput = inputRow.join('').toLowerCase();
			if (mergedInput.length < 5) {
				showMessage = true;
				message = 'Too Short';
				setTimeout(hideMessage, 1000);
				return false;
			}
			if (WORDS.includes(mergedInput)) {
				checkWord();
				// End the game if the word is correct
				if (mergedInput === targetWord) {
					isGameWon = true;
					isGameOver = true;
				} else if (currentAttempt < maxAttempts - 1) {
					currentAttempt++;
					currentInputIndex = 0;
					inputRow = Array(wordLength).fill('');
					attempts[currentAttempt] = [...inputRow];
					(document.getElementById(`word-input-${currentAttempt}-0`) as HTMLInputElement).focus();
				} else {
					isGameOver = true;
				}
			} else {
				showMessage = true;
				message = 'Word not found';
				setTimeout(hideMessage, 1000);
				return false;
			}
		} else if (event.key === ' ') {
			// Prevent the default action for the spacebar
			event.preventDefault();
		}
	};

	const handleVirtualKeyClick = (key: string) => {
		if (isGameOver) return;

		if (key === 'Backspace') {
			if (currentInputIndex > 0) {
				currentInputIndex--;
				inputRow[currentInputIndex] = '';
				attempts[currentAttempt][currentInputIndex] = '';
				(
					document.getElementById(
						`word-input-${currentAttempt}-${currentInputIndex}`
					) as HTMLInputElement
				).value = '';
				(
					document.getElementById(
						`word-input-${currentAttempt}-${currentInputIndex}`
					) as HTMLInputElement
				).focus();
			}
		} else if (key === 'Enter') {
			handleKeyDown(currentInputIndex, new KeyboardEvent('keydown', { key: 'Enter' }));
		} else {
			if (currentInputIndex < wordLength) {
				inputRow[currentInputIndex] = key;
				attempts[currentAttempt][currentInputIndex] = key;
				(
					document.getElementById(
						`word-input-${currentAttempt}-${currentInputIndex}`
					) as HTMLInputElement
				).value = key;
				if (currentInputIndex < wordLength - 1) {
					currentInputIndex++;
				}
				(
					document.getElementById(
						`word-input-${currentAttempt}-${currentInputIndex}`
					) as HTMLInputElement
				).focus();
			}
		}
	};

	const checkWord = () => {
		if (mergedInput.length !== wordLength) return;

		let newColors = Array(wordLength).fill('bg-gray-300'); // Default color for incorrect characters
		let tempTargetWord = targetWord;

		for (let i = 0; i < wordLength; i++) {
			if (inputRow[i].toLowerCase() === targetWord[i].toLowerCase()) {
				newColors[i] = 'bg-green-500'; // Correct placement
				tempTargetWord = tempTargetWord.slice(0, i) + '_' + tempTargetWord.slice(i + 1);
			}
		}

		for (let i = 0; i < wordLength; i++) {
			if (newColors[i] === 'bg-gray-300' && tempTargetWord.includes(inputRow[i].toLowerCase())) {
				newColors[i] = 'bg-yellow-500'; // Correct character but wrong placement
				tempTargetWord = tempTargetWord.replace(inputRow[i].toLowerCase(), '_');
			}
		}

		// Update colors in the attempts array
		colors = [...colors.slice(0, currentAttempt), newColors, ...colors.slice(currentAttempt + 1)];
	};

	const refocus = (event: FocusEvent) => {
		if (!isGameOver && isGameRun) {
			const target = event.relatedTarget as HTMLElement;
			const isCurrentInput = target && target.id.startsWith(`word-input-${currentAttempt}`);
			if (!isCurrentInput) {
				(
					document.getElementById(
						`word-input-${currentAttempt}-${currentInputIndex}`
					) as HTMLInputElement
				).focus();
			}
		}
	};

	onMount(() => {
		isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent);
		if (typeof document !== 'undefined') {
			refocus(new FocusEvent('focusout', { relatedTarget: null }));
			document.addEventListener('focusout', refocus);
		}
	});
	onDestroy(() => {
		if (typeof document !== 'undefined') {
			if (document) document.removeEventListener('focusout', refocus);
		}
	});
</script>

{#if isGameRun}
	{#if showMessage}
		<div class="absolute m-auto h-32 w-72 rounded-lg bg-popover-foreground bg-opacity-90 shadow-lg">
			<div class="flex h-full items-center justify-center">
				<h1 class="text-2xl font-bold text-background">{message}</h1>
			</div>
		</div>
	{/if}
	{#if isGameOver}
		{#if isGameWon}
			<div class="absolute m-auto h-48 w-72 rounded-lg bg-popover bg-opacity-95 p-4 shadow-lg">
				<div class="flex h-full flex-col items-center justify-center">
					<h1 class="text-2xl font-bold text-foreground">Congratulations! You guessed the word.</h1>
					<button class="mt-2 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
						>Play Again</button
					>
				</div>
			</div>
		{:else}
			<div class="absolute m-auto h-48 w-72 rounded-lg bg-popover bg-opacity-95 p-4 shadow-lg">
				<div class="flex h-full flex-col items-center justify-center">
					<h1 class="text-2xl text-foreground">
						You lost! The word is <span class="font-black">{targetWord.toUpperCase()}</span>.
					</h1>
					<button class="mt-2 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
						>Play Again</button
					>
				</div>
			</div>
		{/if}
	{/if}
	{#each Array(maxAttempts) as _, attempt}
		<div class="flex gap-1">
			{#each Array(wordLength) as _, index}
				<input
					id={`word-input-${attempt}-${index}`}
					type="text"
					maxlength="1"
					bind:value={attempts[attempt][index]}
					oninput={(event) => handleInput(index, event)}
					onkeydown={(event) => handleKeyDown(index, event)}
					class={`h-12 w-12 rounded-md border border-gray-300 text-center ${colors[attempt][index]} uppercase caret-transparent focus:outline-none`}
					disabled={isGameOver && attempt >= currentAttempt}
				/>
			{/each}
		</div>
	{/each}
	<VirtualKeyboard onKeyClick={handleVirtualKeyClick} />
{:else}
	<Box class="mb-6 h-40 w-40" />
	<h1 class="mb-3 text-4xl font-bold">Wordle</h1>
	<p class="text-lg">Try guess the hidden word</p>
	<button class="mt-6 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
		>Play Game</button
	>
{/if}
