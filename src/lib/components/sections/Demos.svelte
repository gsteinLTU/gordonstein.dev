<script>
    import ProjectCard from "../ProjectCard.svelte";
</script>

<section class="mt-6 text-left">
    <h2 class="text-2xl font-bold mb-2">Experiments</h2>

    <ProjectCard
        title="PivotSim"
        link="https://gsteinLTU.github.io/PivotSim/"
        linkText="Try It"
        github="https://github.com/gsteinLTU/PivotSIm"
        technologies="JavaScript, Three.js, Vite">
        A browser-based 3D simulator that works out whether a piece of furniture can be
        carried up a staircase. Model the stairwell geometry and furniture dimensions, and
        a solver searches for a collision-free path using simulated annealing or RRT-Connect,
        with a scrubable timeline to replay the trajectory.

        Real world application! My girlfriend and I used this to figure out if we could get a 4 foot diameter table top into our apartment. It worked, and we did!
    </ProjectCard>

    <ProjectCard title="Blackjack RL" link="demos/blackjack-rl" linkText="Try It" technologies="TypeScript, CSS, Tabular Q-Learning">
        A reinforcement learning demo learning a blackjack strategy.
        Q-Learning is a model-free approach (so it has no concept of how the game works at the start), but within a few thousand rounds it should have learned to always hit when under 10, not to hit on a 19 or 20, and be more likely to hit when it has an ace.
    </ProjectCard>

    <ProjectCard
        title="OneNation"
        link="https://gsteinltu.github.io/OneNation/"
        linkText="Try It"
        github="https://github.com/gsteinLTU/OneNation"
        technologies="SvelteKit, Svelte 5, TypeScript">
        A geography guessing game where you identify a secret country from a growing set of
        clues. Each valid guess that fits every clue so far earns you another hint, narrowing
        the field across capital, population, area, borders, region, and more.
    </ProjectCard>
</section>