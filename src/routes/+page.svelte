<script lang="ts">
    import resume from '$lib/assets/resume.pdf';
    import ContactMe from '$lib/components/sections/ContactMe.svelte';
    import AboutMe from '$lib/components/sections/AboutMe.svelte';
    import Links from '$lib/components/sections/Links.svelte';
    import Research from '$lib/components/sections/Research.svelte';
    import Projects from '$lib/components/sections/Projects.svelte';
</script>


<div class="md:w-2/3 w-full bg-white rounded-lg shadow-lg p-8 text-center ml-auto mr-auto mt-20">
    <div class="container">
        <h1 class="text-3xl font-bold">Gordon Stein</h1>
    </div>
    
    <div class="container">
        <h2 class="text-sm text-gray-700 md:text-xl font-semibold">Software Developer & Education Researcher</h2>
    </div>
    <hr class="my-2 mb-4">

    <Links />
    
    <div class="container mt-4 mb-6">
        <a href={resume} class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" download>Download Resume</a>
    </div>

    <AboutMe />
    <Projects />
    <Research />
    <ContactMe />
</div>

<style>
    hr {
        content: '';
        display: block;
        height: 4px;
        background: #15305c;
    }
</style>