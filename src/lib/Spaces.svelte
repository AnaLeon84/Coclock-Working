<script>
    import {link} from "svelte-spa-router"
    const get_spaces = async () => {
      const response = await fetch(import.meta.env.VITE_API_URL + "/items/Space");
      const json = await response.json();
      console.log(json.data)
      return json.data;
    };
  </script>

{#await get_spaces()}
<p>Chargement ...</p>
{:then spaces}
{#each spaces as space}
  <section class="spaces" aria-labelledby="space-title-{space.id}">
    <div>
      <h3 id="space-title-{space.id}">{space.name}</h3>
      <p>Capacité : 10 personnes</p>
      <p>Equipements : {space.equipments}</p>
      <p>Surface : {space.size} m²</p>
      <p>Prix : 100€ par jour</p>
      <a href="/espace/{space.id}" use:link>Détails</a>
    </div>
  </section>
{/each}
{/await}

