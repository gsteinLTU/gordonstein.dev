<script lang="ts">
    import { base } from "$app/paths";
</script>

<h1 class="text-3xl font-bold">RoboScape Online</h1>

<img src="{base}/lidarroad.png" alt="RoboScape Online Screenshot" class="mt-4 rounded-lg shadow-md">

<p class="mt-4">
    RoboScape Online is a networked robotics simulation platform designed for novice programmers and computer science education. It allows users to program virtual robots in a 3D environment using a block-based programming interface, making it accessible for beginners. The platform supports collaborative learning, enabling multiple users to work together in the same virtual space. RoboScape Online is used in educational settings to teach programming concepts, robotics, and STEM skills through interactive simulations.
</p>

<img src="{base}/RoboScapeOnlineWithAPI.png" alt="RoboScape Online Architecture" class="mt-4 rounded-lg shadow-md">

<p class="mt-4">
    The backend of RoboScape Online is built using Rust, providing performance and safety for handling real-time simulations and networking. The frontend is developed with Babylon.js, providing rich 3D graphics and an interactive user experience. The platform leverages WebSockets for real-time communication between clients and the server, ensuring smooth interactions in the shared virtual environment. The platform is containerized with Docker, simplifying deployment and scaling, and simplifying deployment to AWS Elastic Container Service (ECS) for reliable hosting and scalability.
</p>

<img src="{base}/hellocustomblocks.png" alt="RoboScape Online NetsBlox Integration" class="mt-4 rounded-lg shadow-md">

<img src="{base}/helloscenario.png" alt="RoboScape Online NetsBlox Integration" class="rounded-lg shadow-md">

<p>
    An core feature of RoboScape Online is its integration with NetsBlox, a block-based programming language designed for education. This not only makes it easier for beginners to get started with programming, but also allows for deeper interactions. 
    All scenarios available in RoboScape Online are implemented as NetsBlox projects, allowing users to modify and create their own scenarios using the familiar block-based interface. This integration enhances the learning experience by providing a seamless transition between programming and simulation.
</p>

<div class="mt-4 flex flex-col md:flex-row gap-4 w-full">
<a href="https://netsblox.org/roboscape" target="_blank" class="text-blue-600 hover:underline mt-4 inline-block">Visit the RoboScape Website</a>

<a href="../" class="text-blue-600 hover:underline mt-4 inline-block">Return to Main Page</a>
</div>


<style>
    p {
        margin-top: 1rem;
        margin-bottom: 1rem;
        line-height: 1.6;
        text-align: left;
    }
    h1 {
        margin-bottom: 1rem;
        margin-top: 1rem;
    }

    img {
        margin-left: auto;
        margin-right: auto;
        max-width: 60%;
    }

    @media (max-width: 768px) {
        img {
            max-width: 100%;
        }
    }
</style>