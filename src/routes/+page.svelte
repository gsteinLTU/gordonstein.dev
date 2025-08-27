<script lang="ts">
    import resume from '$lib/assets/resume.pdf';
    import AcademiconsGoogleScholarSquare from '~icons/academicons/google-scholar-square';
    import AcademiconsOrcidSquare from '~icons/academicons/orcid-square';
    import RiLinkedinBoxFill from '~icons/ri/linkedin-box-fill';
    import RiGithubFill from '~icons/ri/github-fill';
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

    <section class="mt-6 text-left">
        <h2 class="text-2xl font-bold mb-2">About Me</h2>
        <p class="text-gray-600 mb-3">Full-stack developer and postdoctoral scholar focused on building educational and AI-powered systems. I design and deploy interactive learning tools that integrate large language models and machine learning to create intuitive, accessible learning experiences.</p>
        <ul class="list-disc list-inside text-gray-600 space-y-1">
            <li><strong>Current Role:</strong> Postdoctoral Scholar, LIVE Initiative (Vanderbilt) — educational game development (Svelte frontend, Rust API) and ML/LLM research.</li>
            <li><strong>Skills:</strong> TypeScript, Node.js, Python, Svelte, C#, Rust, TensorFlow.js, Postgres.</li>
            <li><strong>Education:</strong> Ph.D. in Computer Science, Vanderbilt University.</li>
        </ul>
    </section>

    <section class="mt-6 text-left">
        <h2 class="text-2xl font-bold mt-6 mb-4">Contact Me</h2>
        <p class="text-gray-600">Feel free to reach out if you have any questions or would like to collaborate on a project!</p>
        <p class="text-gray-600">You can contact me via email at <a href="mailto:gordon.stein@vanderbilt.edu" class="text-blue-500 hover:underline">gordon.stein@vanderbilt.edu</a>.</p>
    </section>
</div>

<style>
    section {
        margin-bottom: 2rem;
    }

    section h2::after {
        content: '';
        position: relative;
        width: 50%;
        left: -25%;
        margin: 8px auto 0;
    }

    section h2::after, hr {
        content: '';
        display: block;
        height: 4px;
        background: #15305c;
    }
</style>