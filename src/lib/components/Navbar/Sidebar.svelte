<script lang="ts">
	import {
		Home,
		LineChart,
		Package,
		Package2,
		Settings,
		ShoppingCart,
		UsersRound
	} from 'lucide-svelte';

	import SidebarButton from './SidebarButton.svelte';

	let logo = '/assets/images/logo-mini.svg';

	export let path: string;
</script>

<aside
	class="fixed left-0 top-0 z-50 hidden h-screen w-24 flex-col items-center gap-y-4 bg-muted py-5 shadow-lg sm:flex"
>
	<a href="/" class="group flex">
		<img src={logo} alt="Logo" class="h-[4.25rem] w-[4.25rem] px-2" />
		<span class="sr-only">PyramidDB</span>
	</a>
	<div class="flex flex-col items-end gap-y-4 self-end">
		<SidebarButton icon="HomeIconBlack.png" name="Home" href="/" {path} />
		<SidebarButton icon="AvatarIconBlack.png" name="Characters" href="/characters" {path} />
		<SidebarButton icon="RelicsIconBlack.png" name="Relic Scorer" href="/relic-scorer" {path} />
		<SidebarButton
			icon="AchievementIconBlack.png"
			name="Achievements"
			href="/achievements"
			{path}
		/>
		<SidebarButton icon="DataBankIconBlack.png" name="Databank" href="/databank" {path} />
	</div>
</aside>
