<script lang="ts">
	import { page } from '$app/stores';
	import Header from '$lib/components/Navbar/Header.svelte';
	import GameCard from '$lib/components/Playground/GameCard.svelte';
	import { BookA, Box, Cross } from 'lucide-svelte';
	let { children } = $props();
	let path = $derived($page.url.pathname);

	$effect(() => {
		document.title = 'Playground | adhyaksasb';
	});
</script>

<div class="flex min-h-screen w-full flex-col">
	<div class="flex flex-col sm:gap-4 sm:py-4">
		<Header {path} />
		<main class="flex h-full min-w-[300px] flex-col items-center justify-center">
			<div
				class="mb-8 flex h-[40rem] w-full flex-col items-center justify-center gap-3 bg-accent-foreground/10 px-4 text-center"
			>
				{@render children()}
			</div>
			<div class="mb-6 grid grid-cols-2 gap-4 md:grid-cols-3 md:gap-6">
				<GameCard
					icon={BookA}
					href="/playground/verbal-memory"
					title="Verbal Memory"
					body="Keep as many words in short term memory as possible"
				/>
				<GameCard
					icon={Box}
					href="/playground/wordle"
					title="Wordle"
					body="Try guess the hidden word"
				/>
				<GameCard
					icon={Cross}
					href="/playground/tic-tac-toe"
					title="Tic Tac Toe"
					body="Play the Ultimate Tic Tac Toe Showdown!"
				/>
			</div>
		</main>
	</div>
</div>
