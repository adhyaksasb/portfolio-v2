<script lang="ts">
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
</script>

<div
	class="hidden overflow-hidden rounded-xl bg-accent/80 shadow-md dark:border-none lg:col-span-3 lg:row-span-6 lg:block"
>
	{#if isGameRun}
		<div class="board p-2">
			{#each board as cell, index}
				<button
					type="button"
					class="square border-2 border-muted-foreground bg-muted hover:bg-muted-foreground"
					onclick={() => handleClick(index)}
				>
					{cell}
				</button>
			{/each}
		</div>
		{#if winner}
			<div
				class="absolute right-[95px] top-40 w-[200px] rounded-xl bg-background bg-opacity-80 p-3 text-center"
			>
				<p class="text-xl font-bold">
					{winner === player ? 'Player Wins!' : winner === ai ? 'AI Wins!' : "It's a Draw!"}
				</p>
				<button class="rounded" onclick={startGame}>Play Again</button>
			</div>
		{/if}
	{:else}
		<div class="flex h-full items-center justify-center">
			<button class="rounded px-4 py-2" onclick={startGame}>Play Tic Tac Toe</button>
		</div>
	{/if}
</div>

<style>
	.board {
		display: grid;
		grid-template-columns: repeat(3, minmax(0, 1fr));
	}
	.square {
		width: 115px;
		height: 115px;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 2rem;
		cursor: pointer;
	}
</style>
