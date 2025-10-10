<script lang="ts">
    // A simple card component to wrap project details
    import { base } from '$app/paths';
    import RiGithubFill from '~icons/ri/github-fill';
    
    interface Props {
        title: string;
        technologies: string;
        link?: string | null;
        github?: string | null;
        image?: string | null;
        children?: any;
    }

    let { title, technologies, link = null, github = null, image = null, children } : Props = $props();
</script>

<article class="bg-white rounded-lg shadow-lg p-6 mb-6 flex flex-row gap-2">
    <div>
        <h3 class="text-xl font-semibold mb-4">{title} 

            {#if github}
                <a href={github} class="text-gray-600 hover:text-black inline-block" target="_blank" rel="noopener noreferrer" aria-label="GitHub Repository" title="GitHub Repository">
                    <RiGithubFill class="inline w-5 h-5 ml-2" />
                </a>
            {/if}
        </h3>
        <p class="text-gray-600 mb-4">Technologies Used: {technologies}</p>
        {@render children?.()}
        {#if link}
            <p>
                <a href={link} class="text-blue-500 hover:underline mt-4 inline-block" target="_blank" rel="noopener noreferrer">Learn More</a>
            </p>
        {/if}
    </div>
    {#if image}
        <figure class="mb-4">
            <img src={(image?.startsWith('/') ? base + image : image)} alt={title} class="w-full h-48 object-cover rounded-md mb-4" />
        </figure>
    {/if}
</article>

<style>
    article {
        transition: transform 0.2s;
    }
    figure {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        max-width: 320px;
        min-width: 180px;
        margin-left: auto;
        margin-right: auto;
    }
    figure img {
        width: 100%;
        height: auto;
        max-height: 200px;
        object-fit: cover;
        border-radius: 0.5rem;
        display: block;
    }
    @media (max-width: 640px) {
        article {
            flex-direction: column;
        }
        figure {
            max-width: 100%;
            min-width: 0;
        }
        figure img {
            max-height: 160px;
        }
    }
</style>