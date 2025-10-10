<script lang="ts">
    import resume from '$lib/assets/resume.pdf';
    import ContactMe from '$lib/components/sections/ContactMe.svelte';
    import AboutMe from '$lib/components/sections/AboutMe.svelte';
    import Research from '$lib/components/sections/Research.svelte';
    import Projects from '$lib/components/sections/Projects.svelte';
</script>

<div class="container mt-4 mb-6">
    <a href={resume} class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" download>Download Resume</a>
</div>

<AboutMe />
<Projects />
<Research />
<ContactMe />
