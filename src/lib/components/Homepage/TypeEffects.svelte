<script lang="ts">
	import { gsap } from 'gsap';

	let words: string[] = [
		'Fullstack Developer',
		'Frontend Developer',
		'Backend Developer',
		'UI/UX Designer'
	];

	const typewriterEffects = async () => {
		const { TextPlugin } = await import('gsap/TextPlugin');
		gsap.registerPlugin(TextPlugin);

		gsap.to('#cursor', {
			opacity: 0,
			repeat: -1,
			yoyo: true,
			ease: 'power2.inOut'
		});
		let tlMaster = gsap.timeline({ repeat: -1 });
		words.forEach((word) => {
			let tlText = gsap.timeline({ repeat: 1, yoyo: true, repeatDelay: 1 });
			tlText.to('#typewriter', { duration: 1, text: word });
			tlMaster.add(tlText);
		});
	};

	$effect(() => {
		typewriterEffects();
	});
</script>

<div
	class="col-span-5 row-span-3 row-start-4 min-h-20 overflow-hidden rounded-xl bg-accent/80 md:col-span-4 md:row-span-2 md:row-start-auto lg:col-span-3 lg:row-span-2"
>
	<div class="flex h-full items-center justify-center text-sm md:text-lg">
		<p>I'm a&nbsp;</p>
		<span id="typewriter"></span><span id="cursor" class="ml-1 inline-block h-4 w-1">_</span>
	</div>
</div>
