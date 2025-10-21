<script>
    import { base } from "$app/paths";
    import ProjectCard from "../ProjectCard.svelte";

</script>

<section class="mt-6 text-left">
    <h2 class="text-2xl font-bold mb-2">Assorted Demos</h2>

    <ProjectCard title="Blackjack RL" link={base + "/demos/blackjack-rl"} technologies="TypeScript, CSS, Tabular Q-Learning">
        A reinforcement learning demo learning a blackjack strategy.
        Q-Learning is a model-free approach, but within a few thousand rounds it should have learned to always hit when under 10, not to hit on a 19 or 20, and be more likely to hit when it has an ace.
    </ProjectCard>
</section>