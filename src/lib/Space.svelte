<script>
    import Login from "./Login.svelte";
    let token;
    let newComment = { title: "", description: "", rate: 0 };
  
    export let params = {};
    const space_id = params.id;
  
    const get_space = async (id) => {
      // Appel de la requête
      const response = await fetch(
        import.meta.env.VITE_API_URL + "/items/Space/" + id,
      );
      // Extraction du json de la réponse
      const result = await response.json();
      //Extraction et retour de la liste
      return result.data;
    };
    const load_comments = async () => {
      token = localStorage.getItem("token");
      const comments = await get_comments(token);
      return comments;
    };
    // Fonction de récupération de la liste des ids de résa
  
    // Fonction de récupération de la liste des commentaires
    const get_comments = async (token) => {
      let endpoint = import.meta.env.VITE_API_URL + "/items/Comments?";
      endpoint += "filter[space_id][_eq]=" + space_id;
      // Appel de la requête
      console.log(token);
      const response = await fetch(endpoint, {
        headers: {
          Authorization: "Bearer " + token,
        },
      });
      // Extraction du json de la réponse
      const result = await response.json();
      console.log(result);
      //Extraction et retour de la liste
      return result.data;
    };
  
    const handleSubmit = async (token) => {
      const { title, description, rate } = newComment;
      const result = await fetch(
        import.meta.env.VITE_API_URL + "/items/Comments",
        {
          method: "POST",
          headers: {
            "Content-type": "application/json",
            Authorization: "Bearer " + token,
          },
          body: JSON.stringify({
            space_id,
            title,
            description,
            rate,
          }),
        },
      );
      location.reload();
    };
  </script>
  
  <h1 id="title1">Les espaces</h1>
  {#await get_space(space_id)}
    <p>Récupération des données de l'espace</p>
  {:then space}
    <h2>Informations</h2>
    <ul>
      <li>Nom : {space.name}</li>
      <li>Surface : {space.size}</li>
      <li>Ville : {space.location}</li>
    </ul>
    {#if token !== null}
      <h2>Commentaires</h2>
      {#await load_comments()}
        <p>Récupération des commentaires</p>
      {:then comments}
        <ul>
          {#each comments as comment}
            <li>
              <p>{comment.title}</p>
              <p>{comment.description}</p>
              <p>{comment.rate}</p>
            </li>
          {/each}
        </ul>
        <form
          on:submit|preventDefault={() => handleSubmit(token)}
          aria-label="nouveaux commentaire"
        >
          <label for="title">Titre</label>
          <input
            required
            type="text"
            name="title"
            id="title"
            placeholder="Lache ton titre"
            bind:value={newComment.title}
          />
  
          <label for="comment">Commentaire</label>
          <textarea
            required
            name="comment"
            id="comment"
            placeholder="Lache ton com"
            bind:value={newComment.description}
          />
          <label for="rate">Note</label>
          <input
            required
            min="0"
            max="10"
            type="number"
            name="rate"
            id="rate"
            bind:value={newComment.rate}
          />
  
          <input type="submit" value="Commenter" />
        </form>
        <div>{JSON.stringify(newComment)}</div>
      {/await}
    {:else}Connectez-vous pour voir les commentaires
      <Login reload={true} />{/if}
  {/await}