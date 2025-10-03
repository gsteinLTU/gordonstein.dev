<script lang="ts">
	import '../app.css';
	
	let { children } = $props();
</script>

<svelte:head>
	<title>Gordon Stein - Software Developer & CS Education Researcher</title>
	<link rel="icon" href="./fav.png" />

	<!-- Primary Meta Tags -->
	<meta name="description" content="Gordon Stein's personal website. Software Developer & Computer Science Education Researcher." />
	<meta name="author" content="Gordon Stein" />

	<!-- Open Graph / Facebook -->
	<meta property="og:title" content="Gordon Stein - Software Developer & CS Education Researcher" />
	<meta property="og:description" content="Gordon Stein's personal website. Software Developer & Computer Science Education Researcher." />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://gordonstein.dev" />
</svelte:head>

{@render children?.()}
