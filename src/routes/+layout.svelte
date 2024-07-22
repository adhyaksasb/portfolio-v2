<script lang="ts">
	import '../app.css';

	import { ModeWatcher } from 'mode-watcher';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href="/assets/logo-mini.svg" />
</svelte:head>

<ModeWatcher />
{@render children()}
