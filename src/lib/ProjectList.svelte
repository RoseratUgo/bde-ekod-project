<script>
    import { onMount } from 'svelte';
    let studentProjects = []; // Initialiser comme tableau vide

    onMount(async () => {
        const response = await fetch('https://bdeapi.ugoroserat.com/projects');
        if (response.ok) {
            let projects = await response.json();
            // Ajuster les URLs des images pour chaque projet
            studentProjects = projects.map(project => {
                if (project.image && !project.image.startsWith('http')) {
                    // Ajouter le préfixe seulement si l'URL de l'image n'est pas absolue
                    project.image = `https://github.com${project.image.startsWith('/') ? '' : '/'}${project.image}`;
                }
                return project;
            });
        } else {
            console.error('Erreur lors de la récupération des projets', response.statusText);
        }
    });
</script>

<main>
    <div class="projects-container">
        {#each studentProjects as project}
            <a href={project.url} target="_blank" rel="noopener noreferrer" class="project-box-link">
                <div class="project-box">
                    <img src={project.image} alt={`Image de ${project.title}`} class="project-image"/>
                    <h3>{project.title.toUpperCase()}</h3>
                    <p>{project.description}</p>
                    <span class="project-name">{project.name}</span>
                </div>
            </a>
        {/each}
    </div>
</main>

<style>
    .projects-container {
        display: flex;
        flex-wrap: wrap;
        gap: 20px;
        justify-content: center;
        padding: 20px;
    }

    .project-box-link {
        text-decoration: none; /* Enlève le soulignement des liens */
        color: inherit; /* Hérite la couleur du texte de l'élément parent */
        width: 300px; /* Assure que le lien ait la même largeur que la boîte */
    }

    .project-box {
        border: 2px solid #646cff;
        border-radius: 10px;
        padding: 20px;
        text-align: center;
        transition: transform 0.3s ease;
        overflow: hidden;
    }

    .project-box:hover {
        transform: translateY(-5px);
        border-color: #ff3e00;
    }

    .project-image {
        width: 100%;
        height: auto;
        border-radius: 10px;
        margin-bottom: 10px;
    }

    h3 {
        text-transform: uppercase; /* Met le titre en majuscules */
        margin: 0 0 10px 0; /* Ajuste l'espacement autour du titre */
    }

    .project-name {
        display: block; /* Rend le nom du projet un bloc pour un meilleur espacement */
        font-weight: bold;
        margin-top: 10px;
    }
</style>
