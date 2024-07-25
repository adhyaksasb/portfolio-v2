<script lang="ts">
	import { cn } from '$lib/utils';

	let { icon, name, href, path }: any = $props();

	let isHovered: boolean = $state(false);

	let slicedHref = $derived('/' + href.split('/')[1]);
	let slicedPath = $derived('/' + path.split('/')[1]);
</script>

<div
	class={cn(
		slicedPath === slicedHref
			? 'relative flex rounded-l-xl bg-background before:absolute before:-top-8 before:right-0 before:h-8 before:w-4 before:rounded-br-xl before:shadow-inverse-top after:absolute after:-bottom-8  after:right-0 after:h-8 after:w-4 after:rounded-tr-xl after:shadow-inverse-bottom  before:dark:shadow-inverse-top-dark after:dark:shadow-inverse-bottom-dark'
			: 'flex'
	)}
>
	<!-- svelte-ignore a11y_mouse_events_have_key_events -->
	<a
		{href}
		class={cn(
			'my-4 ml-3 mr-4 p-4',
			slicedPath === slicedHref ? 'rounded-xl bg-primary shadow-custom' : 'rounded-xl'
		)}
		onmouseover={() => (isHovered = true)}
		onmouseout={() => (isHovered = false)}
	>
		<img
			src="https://pyramidb.s3.ap-southeast-3.amazonaws.com/img/starRail/icon/sign/{icon}"
			alt="achievements"
			class={cn(
				'h-6 w-6 fill-current',
				slicedPath === slicedHref ? 'whiteFilter' : 'primaryFilter'
			)}
		/>
	</a>
	<div
		class={cn(
			'text-text-primary absolute left-28 z-10 mt-4 text-nowrap rounded-xl bg-muted p-4 font-semibold shadow-lg',
			isHovered ? 'block' : 'hidden'
		)}
	>
		<p>{name}</p>
	</div>
</div>
