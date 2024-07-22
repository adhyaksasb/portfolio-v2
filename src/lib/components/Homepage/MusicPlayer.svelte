<script lang="ts">
	import Fa from 'svelte-fa';
	import { faPlay, faPause } from '@fortawesome/free-solid-svg-icons';
	import { cn } from '$lib/utils';

	let isPlayed: boolean = $state(false);

	const handleClick = () => {
		isPlayed = !isPlayed;
		console.log(isPlayed);
	};
</script>

<div
	class="col-span-2 col-start-6 row-span-3 row-start-4 rounded-xl bg-muted/40 md:col-span-3 md:col-start-auto md:row-span-4 md:row-start-auto md:block lg:col-span-2 lg:row-span-5 lg:block"
>
	<div class="flex h-full flex-col items-center justify-center lg:justify-normal">
		<div
			class={cn(
				"h-[89%] w-[89%] overflow-hidden rounded-lg bg-slate-400 bg-[url('/assets/images/coding.jpg')] bg-cover ring-2 ring-slate-700 ring-offset-4 ring-offset-[#21232b] transition-transform md:h-[70%] md:w-[70%] lg:mt-8 lg:h-[54%]",
				isPlayed ? 'scale-110' : ''
			)}
		>
			<div
				class="hidden h-full w-full flex-col items-center justify-center rounded-lg opacity-0 backdrop-blur-lg backdrop-brightness-50 transition-all duration-300 hover:opacity-100 md:flex"
			>
				<h1 class="select-none text-xl font-bold">Chilly</h1>
				<div class="select-none text-xs font-light">NIKI</div>
			</div>
		</div>
		<button onclick={handleClick} class="hidden pt-9 lg:block">
			{#if isPlayed}
				<Fa icon={faPause} size="3.5x" />
			{:else}
				<Fa icon={faPlay} size="3.5x" />
			{/if}
		</button>
	</div>
</div>
