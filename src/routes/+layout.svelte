<script lang="ts">
    import { base } from '$app/paths';
    import Links from '$lib/components/sections/Links.svelte';
	import '../app.css';
	
	let { children } = $props();
</script>

<svelte:head>
	<title>Gordon Stein - Research Engineer | Applied AI, Software Systems & Computer Science Education</title>
	<link rel="icon" href="{base}/fav.png" />

	<!-- Primary Meta Tags -->
	<meta name="description" content="Gordon Stein's personal website. Research Engineer | Applied AI, Software Systems & Computer Science Education." />
	<meta name="author" content="Gordon Stein" />

	<!-- Open Graph / Facebook -->
	<meta property="og:title" content="Gordon Stein - Research Engineer | Applied AI, Software Systems & Computer Science Education" />
	<meta property="og:description" content="Gordon Stein's personal website. Research Engineer | Applied AI, Software Systems & Computer Science Education." />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://gordonstein.dev" />
</svelte:head>



<div class="md:w-2/3 w-full bg-white rounded-lg shadow-lg p-8 text-center ml-auto mr-auto mt-20">
    <div class="container">
        <h1 class="text-3xl font-bold"><a href="{base}/">Gordon Stein</a></h1>
    </div>
    
    <div class="container">
        <h2 class="text-sm text-gray-700 md:text-xl font-semibold">Research Engineer | Applied AI, Software Systems & Computer Science Education</h2>
    </div>
    <hr class="my-2 mb-4">

	<Links />

{@render children?.()}

</div>


<style>
    hr {
        content: '';
        display: block;
        height: 4px;
        background: #15305c;
    }
</style>