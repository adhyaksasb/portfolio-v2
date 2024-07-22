<script lang="ts">
	import PreLoading from '$lib/components/Loading/PreLoading.svelte';
	import { gsap } from 'gsap';
	import { Moon, Sun } from 'lucide-svelte';
	import { toggleMode } from 'mode-watcher';
	import { Button } from '$lib/components/ui/button/index.js';
	import SocialLinks from '$lib/components/Homepage/SocialLinks.svelte';
	import MusicPlayer from '$lib/components/Homepage/MusicPlayer.svelte';
	import PortfolioCard from '$lib/components/Homepage/PortfolioCard.svelte';
	import GameplayCard from '$lib/components/Homepage/GameplayCard.svelte';
	import GridEffects from '$lib/components/Homepage/GridEffects.svelte';
	import CentralLogo from '$lib/components/Homepage/CentralLogo.svelte';
	import ProfileCard from '$lib/components/Homepage/ProfileCard.svelte';
	import TypeEffects from '$lib/components/Homepage/TypeEffects.svelte';
	import SkillSet from '$lib/components/Homepage/SkillSet.svelte';
	import TextLogo from '$lib/components/Homepage/TextLogo.svelte';

	let logo = '/assets/logo-mini.svg';

	$effect(() => {
		document.title = 'Adhyaksa Setiabudi | adhyaksasb';

		gsap.fromTo(
			'.loading-page',
			{ opacity: 1 },
			{
				opacity: 0,
				display: 'none',
				duration: 1.3,
				delay: 3.5
			}
		);

		gsap.fromTo(
			'.logo-name',
			{
				y: 50,
				opacity: 0
			},
			{
				y: 0,
				opacity: 1,
				duration: 2,
				delay: 0.5
			}
		);
	});
</script>

<div class="absolute right-0 top-0">
	<Button on:click={toggleMode} variant="outline" size="icon">
		<Sun
			class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
		/>
		<Moon
			class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
		/>
		<span class="sr-only">Toggle theme</span>
	</Button>
</div>

<div
	class="grid-rows-13 absolute bottom-0 left-0 right-0 top-0 m-auto grid h-[800px] max-w-[450px] grid-cols-7 gap-3 overflow-y-hidden rounded-xl p-4 drop-shadow-lg md:h-[61.17vw] md:max-h-[520px] md:max-w-[850px] md:grid-cols-10 md:grid-rows-8 lg:h-[46.774vw] lg:max-h-[580px] lg:max-w-[1240px] lg:grid-cols-10 lg:grid-rows-9"
>
	<!-- <div
		class="bg-muted/40 row-span-13 flex flex-col items-center justify-evenly rounded-xl md:row-span-8 lg:row-span-9"
	>
		<button class="flex h-20 w-20 items-center justify-center rounded-lg"
			><Fa icon={faHouse} size="3.3x" /></button
		>
		<button class="flex h-20 w-20 items-center justify-center rounded-lg"
			><Fa icon={faUser} size="3.3x" /></button
		>
		<button class="flex h-20 w-20 items-center justify-center rounded-lg"
			><Fa icon={faSquarePollHorizontal} size="3.3x" /></button
		>
		<button class="flex h-20 w-20 items-center justify-center rounded-lg"
			><Fa icon={faCode} size="3.3x" /></button
		>
		<button class="flex h-20 w-20 items-center justify-center rounded-lg"
			><Fa icon={faLaptopCode} size="3.3x" /></button
		>
	</div> -->
	<SocialLinks />
	<MusicPlayer />
	<PortfolioCard />
	<GameplayCard />
	<GridEffects />
	<CentralLogo />
	<ProfileCard />
	<TypeEffects />
	<TextLogo />
	<SkillSet />
</div>
<!-- <PreLoading /> -->
