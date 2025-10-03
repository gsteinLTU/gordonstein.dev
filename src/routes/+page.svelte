<script lang="ts">
    import resume from '$lib/assets/resume.pdf';
    import AcademiconsGoogleScholarSquare from '~icons/academicons/google-scholar-square';
    import AcademiconsOrcidSquare from '~icons/academicons/orcid-square';
    import RiLinkedinBoxFill from '~icons/ri/linkedin-box-fill';
    import RiGithubFill from '~icons/ri/github-fill';
    import ContactMe from '$lib/components/sections/ContactMe.svelte';
    import AboutMe from '$lib/components/sections/AboutMe.svelte';
</script>


<div class="md:w-2/3 w-full bg-white rounded-lg shadow-lg p-8 text-center ml-auto mr-auto mt-20">
    <div class="container">
        <h1 class="text-3xl font-bold">Gordon Stein</h1>
    </div>
    
    <div class="container">
        <h2 class="text-sm text-gray-700 md:text-xl font-semibold">Software Developer & Education Researcher</h2>
    </div>
    <hr class="my-2 mb-4">

    <div class="flex flex-row w-full justify-center items-center space-x-4 mt-3">
        <a href="https://www.linkedin.com/in/gordon-stein" target="_blank" rel="noopener noreferrer" aria-label="LinkedIn" title="LinkedIn" class="text-gray-700 hover:text-blue-700">
            <RiLinkedinBoxFill class="w-7 h-7" />
        </a>
        <a href="https://github.com/gsteinLTU" target="_blank" rel="noopener noreferrer" aria-label="GitHub" title="GitHub" class="text-gray-700 hover:text-black">
            <RiGithubFill class="w-7 h-7" />
        </a>
        <a href="https://scholar.google.com/citations?user=eZFZqPoAAAAJ" target="_blank" rel="noopener noreferrer" aria-label="Google Scholar" title="Google Scholar" class="text-gray-700 hover:text-blue-600">
            <AcademiconsGoogleScholarSquare class="w-7 h-7" />
        </a>
        <a href="https://orcid.org/0000-0002-1422-2814" target="_blank" rel="noopener noreferrer" aria-label="ORCID" title="ORCID" class="text-gray-700 hover:text-green-600">
            <AcademiconsOrcidSquare class="w-7 h-7" />
        </a>
    </div>
    
    <div class="container mt-4 mb-6">
        <a href={resume} class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" download>Download Resume</a>
    </div>

    <AboutMe />
    <ContactMe />
</div>