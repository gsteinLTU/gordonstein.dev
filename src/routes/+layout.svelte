<script lang="ts">
	import '../app.css';
	
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href="./fav.png" />
</svelte:head>

{@render children?.()}
