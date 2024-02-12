<script>
    import { onMount } from 'svelte';
    let studentProjects = [];
    let projectOfTheMonth = {};

    onMount(async () => {
        const response = await fetch('https://bdeapi.ugoroserat.com/projects');
        if (response.ok) {
            let projects = await response.json();
            studentProjects = projects.map(project => {
                if (project.image && !project.image.startsWith('http')) {
                    project.image = `https://github.com${project.image.startsWith('/') ? '' : '/'}${project.image}`;
                }
                return project;
            });
            projectOfTheMonth = projects.find(p => p.id === 2);
        } else {
            console.error('Erreur lors de la récupération des projets', response.statusText);
        }
    });
</script>

<main class="bg-gray-900 text-white">
    <div class="navbar bg-gray-800 p-5 flex items-center">
        <img src="/logo.svg" alt="" class="h-15">
    </div>
    <div class="projects-container p-4">
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
            <!-- Projet du mois -->
            <div class="lg:col-span-1 lg:row-span-2 bg-gray-700 shadow-lg rounded-lg overflow-hidden flex flex-col">
                <div class="text-center py-2 bg-gray-600 text-white font-bold uppercase">Projet du mois</div>
                <img src={projectOfTheMonth.image} alt={`Image de ${projectOfTheMonth.title}`} class="w-full object-cover" style="height: 300px;"/>
                <div class="p-4 flex-grow">
                    <h3 class="text-lg font-semibold">{projectOfTheMonth.title}</h3>
                    <p>{projectOfTheMonth.description}</p>
                </div>
                <div class="p-4 bg-gray-800 flex justify-between items-center">
                    <p class="text-sm">{projectOfTheMonth.name}<br>{projectOfTheMonth.classname}</p>
                    <a href={projectOfTheMonth.url} target="_blank"
                       class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 transition duration-300">Découvrir</a>
                </div>
            </div>
            <!-- Autres projets -->
            {#each studentProjects as project}
                <div class="project-box bg-gray-700 shadow-lg rounded-lg overflow-hidden">
                    <img src={project.image} alt={`Image de ${project.title}`} class="w-full"/>
                    <div class="p-4">
                        <h3 class="text-lg font-semibold">{project.title}</h3>
                        <p>{project.description}</p>
                    </div>
                    <div class="p-4 bg-gray-800 flex justify-between items-center">
                        <p class="text-sm">{project.name}<br>{project.classname}</p>
                        <a href={project.url} target="_blank"
                           class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700 transition duration-300">Découvrir</a>
                    </div>
                </div>
            {/each}
        </div>
    </div>
</main>
