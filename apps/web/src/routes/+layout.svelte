<script lang="ts">
	import '../app.css';
	import favicon from '$lib/assets/favicon.svg';
  import { ModeWatcher, setMode } from "mode-watcher";
	import { onMount } from "svelte";

	let { children } = $props();

	onMount(() => {
		setMode('dark')
	})
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

<ModeWatcher />
{@render children?.()}
