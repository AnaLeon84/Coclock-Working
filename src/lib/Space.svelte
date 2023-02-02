<script>
    export let space_id;

    const get_space = async (id) => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/Spaces/" + id);
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    }

    const load_comments = async () => {
        const token = await login();
        const resa_ids = await get_resa_ids(token, space_id);
        const comments = await get_comments(token, resa_ids);
        return comments
    }

    // Fonction de récupération d'un token de connexion
    const login = async () => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/auth/login", {
            method: "POST",
            headers: {
                "Content-type": "application/json"
            },
            body: JSON.stringify({
                email: "client@oclock.io",
                password: "1234"
            })
        });
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour du token
        return result.data.access_token;
    }

    // Fonction de récupération de la liste des ids de résa
    const get_resa_ids = async (token, id) => {
        // Création du endpoint avec filtre et fields
        let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/Reservations?";
        endpoint += "filter[space_id][_eq]=" + id;
        endpoint += "&fields=space_id";

        // Appel de la requête
        const response = await fetch(endpoint, {
            headers: {          
                'Authorization': 'Bearer ' + token,
            }
        });
        // Extraction du json de la réponse
        const result = await response.json();

        //Extraction des ids pour les mettre dans un tableau
        let ids = [];
        result.data.forEach(item => {
            ids.push(item.space_id);
        });
        return ids;
    }

    // Fonction de récupération de la liste des commentaires
    const get_comments = async (token, resa_ids) => {
        let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/Comments?";
        endpoint += "filter[resa_id][_in]=" + resa_ids.join(',');

        // Appel de la requête
        const response = await fetch(endpoint, {
            headers: {          
                'Authorization': 'Bearer ' + token,
            }
        });
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    }
</script>


<h1 id="title1">Les espaces</h1>

{#await get_space(space_id)}
    <p>Récupération des données de l'espace</p>
{:then data} 
    <h2>Informations</h2>
    <ul>
        <li>Nom : {data.name}</li>
        <li>Surface : {data.size}</li>
        <li>Equipements : {data.equipments}</li>
    </ul>

    {#await load_comments()}
        <p>Récupération des commentaires</p>
    {:then comments} 
        <h2>Commentaires</h2>
        <ul>
        {#each comments as comment}
            <li>{comment.title}</li>
        {/each}
        </ul>
    {/await}
{/await}