<script lang="ts">
	import { gsap } from 'gsap';
	import { getRandom } from '$lib/utils';

	let characters = 'ADHYAKSASB'.split('');

	const onMouseEnter = (event: Event) => {
		const element = event.target;
		gsap.to(element, {
			x: getRandom(-30, 30),
			y: getRandom(-30, 30),
			rotation: getRandom(-30, 30),
			scale: getRandom(0.8, 1.5),
			duration: 0.3,
			ease: 'power1.out'
		});
	};

	const onMouseLeave = (event: Event) => {
		const element = event.target;
		gsap.to(element, {
			x: 0,
			y: 0,
			rotation: 0,
			scale: 1,
			duration: 0.3,
			ease: 'power1.out'
		});
	};
</script>

<div
	class="row-span-4 rounded-xl bg-muted/40 shadow-lg md:col-span-2 md:row-span-2 lg:col-span-4 lg:row-span-2"
>
	<div class="flex h-full items-center justify-center p-4 text-center">
		<div class="flex shrink flex-col md:flex-row">
			{#each characters as char, i}
				<span
					role="button"
					tabindex="0"
					class="text-md font-bold lg:text-2xl"
					onmouseenter={onMouseEnter}
					onmouseleave={onMouseLeave}
					aria-label={`Character ${char}`}>{char}</span
				>
			{/each}
		</div>
	</div>
</div>
