<script>
    import ProjectCard from "../ProjectCard.svelte";

</script>
<section class="mt-6 text-left">
    <h2 class="text-2xl font-bold mb-2">Selected Projects</h2>

<ProjectCard title="RoboScape Online" technologies="TypeScript, Rust, WebSockets, Docker, AWS" image="/robot.png">
    RoboScape Online is a networked educational robotics simulation platform designed for novice programmers. 
    It enables students to control virtual robots in a 3D environment, facilitating collaborative learning and remote education. 
    The platform integrates with a block-based programming language, NetsBlox, to make robotics accessible to beginners.
</ProjectCard>

<ProjectCard title="kidGPT: Language Models for Young Learners" technologies="Python, Torch">
    kidGPT is a research project working to make large language models (LLMs) more accessible for young learners while avoiding potential harms from inappropriate outputs. By training LLMs on age-appropriate, licensed data, we aim to create an ecosystem of transparent, safe tools to allow children to explore how large language models are trained and used.
</ProjectCard>

<ProjectCard title="AI-Powered Research Interview Platform" technologies="TypeScript, Svelte, SvelteKit, Supabase, Postgres">
    We wanted to utilize a freshman course for a research study, but needed a way to conduct interviews at scale while also reducing bias. This platform uses AI to conduct anonymous semi-structured interviews with students, allowing researchers to gather qualitative data without needing to perform interviews individually.
</ProjectCard>

<ProjectCard title="AI Ethics Educational Game" technologies="TypeScript, Svelte, Rust, TensorFlow.js, Postgres, AWS" image="/spot.png">
    Young students learn about computational thinking, machine learning, and AI ethics through an interactive game. Students train and test machine learning models in a custom age-appropriate interface, while also considering the ethical implications of AI technologies. 
</ProjectCard>

<ProjectCard title="BloxBuddy - AI Assistant for Block-Based Programming" technologies="TypeScript, OpenAI API">
    BloxBuddy is an AI-powered assistant designed to help students learn block-based programming in NetsBlox. Students are guided through a conversation designed to keep them on track, while also being helpful without sacrificing the learning process by giving away answers.
</ProjectCard>

<ProjectCard title="Gesture Recognition Blocks" technologies="TypeScript, Svelte, TensorFlow.js">
    Time series gesture recognition blocks for Scratch, using classification models trained with TensorFlow.js in a custom web interface. These blocks allow students to create interactive applications that respond to dynamic body movements.
</ProjectCard>
</section>


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

    section h2::after {
        content: '';
        display: block;
        height: 4px;
        background: #15305c;
    }
</style>