<script lang="ts">
	import { cn } from '$lib/utils';
	import { Cross } from 'lucide-svelte';

	type Board = (string | null)[];
	let board: Board = $state(Array(9).fill(null));
	let isPlayerTurn: boolean = $state(true);
	let winner: string | null = $state(null);
	let isGameRun: boolean = $state(false);

	const player = 'X';
	const ai = 'O';

	const checkWinner = (board: (string | null)[]): string | null => {
		const winningCombinations = [
			[0, 1, 2],
			[3, 4, 5],
			[6, 7, 8],
			[0, 3, 6],
			[1, 4, 7],
			[2, 5, 8],
			[0, 4, 8],
			[2, 4, 6]
		];

		for (let combo of winningCombinations) {
			const [a, b, c] = combo;
			if (board[a] && board[a] === board[b] && board[a] === board[c]) {
				return board[a];
			}
		}

		if (board.every((cell) => cell !== null)) {
			return 'draw'; // No winner and no empty spaces left
		}

		return null;
	};

	const minimax = (
		newBoard: (string | null)[],
		depth: number,
		alpha: number,
		beta: number,
		currentPlayer: string
	): { index: number; score: number } => {
		const emptySquares = newBoard.reduce<number[]>((acc, val, idx) => {
			if (!val) acc.push(idx);
			return acc;
		}, []);

		const winner = checkWinner(newBoard);
		if (winner === ai) return { score: 10 - depth, index: -1 };
		if (winner === player) return { score: depth - 10, index: -1 };
		if (emptySquares.length === 0) return { score: 0, index: -1 };

		let bestMove = { index: -1, score: currentPlayer === ai ? -Infinity : Infinity };

		for (const idx of emptySquares) {
			newBoard[idx] = currentPlayer;

			const result = minimax(newBoard, depth + 1, alpha, beta, currentPlayer === ai ? player : ai);
			newBoard[idx] = null;

			if (currentPlayer === ai) {
				if (result.score > bestMove.score) {
					bestMove = { index: idx, score: result.score };
					alpha = Math.max(alpha, result.score);
				}
			} else {
				if (result.score < bestMove.score) {
					bestMove = { index: idx, score: result.score };
					beta = Math.min(beta, result.score);
				}
			}

			if (beta <= alpha) break;
		}

		return bestMove;
	};

	const handleClick = (index: number) => {
		if (!board[index] && isPlayerTurn && !winner) {
			board[index] = player;
			isPlayerTurn = false;

			winner = checkWinner(board);
			if (!winner) {
				// Simulate AI thinking time
				setTimeout(() => {
					if (!winner) {
						const bestMove = minimax(board, 0, -Infinity, Infinity, ai);
						if (bestMove.index !== -1) {
							board[bestMove.index] = ai;
							isPlayerTurn = true;
						}
						winner = checkWinner(board);
					}
				}, 100); // Adjust timeout as needed
			}
		}
	};

	const startGame = () => {
		board = Array(9).fill(null);
		isPlayerTurn = true;
		winner = null;
		isGameRun = true;
	};
	const resetGame = () => {
		board = Array(9).fill(null);
		isPlayerTurn = true;
		winner = null;
	};
</script>

<div class="flex min-h-[36rem] w-full flex-col items-center justify-center">
	{#if isGameRun}
		{#if winner}
			<p class="mt-4 text-xl font-bold">
				{winner === player ? 'Player Wins!' : winner === ai ? 'AI Wins!' : "It's a Draw!"}
			</p>
		{/if}
		<div class="board">
			{#each board as cell, index}
				<button
					type="button"
					class={cn(
						'square hover:bg-muted-foreground',
						winner ? 'cursor-not-allowed bg-muted-foreground' : 'bg-muted'
					)}
					onclick={() => handleClick(index)}
				>
					{cell}
				</button>
			{/each}
		</div>
		{#if winner}
			<button class="mt-2 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
				>Play Again</button
			>
		{:else}
			<button class="mt-4 rounded bg-blue-500 px-4 py-2 text-white" onclick={resetGame}
				>Reset</button
			>
		{/if}
	{:else}
		<Cross class="mb-6 h-44 w-44" />
		<h1 class="mb-3 text-4xl font-bold">Tic Tac Toe</h1>
		<p class="text-lg">Play the Ultimate Tic Tac Toe Showdown and takedown this powerful AI</p>
		<button class="mt-6 rounded bg-blue-500 px-4 py-2 text-white" onclick={startGame}
			>Play Game</button
		>
	{/if}
</div>

<style>
	.board {
		display: grid;
		grid-template-columns: repeat(3, minmax(0, 1fr));
		grid-gap: 1rem;
		width: 300px;
		margin: 2rem auto;
	}
	.square {
		width: 100px;
		height: 100px;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 2px solid #000;
		font-size: 2rem;
	}
</style>
