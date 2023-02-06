<script>
  import Router from "svelte-spa-router";
  import Home from "./lib/Home.svelte";
  import Login from "./lib/Login.svelte";
  import Spaces from "./lib/Spaces.svelte";
  import Space from "./lib/Space.svelte";

  import Menu from "./lib/Menu.svelte";
  import Stats from "./lib/Stats.svelte";

const routes={
  '/':Home,
  '/login':Login,
  '/espaces':Spaces,
  '/espace/:id':Space,
  '*':Home
}

</script>


<header>
  <Menu />
  <!-- Bandeau contenant l'image -->
  <div class="bandeau" />
</header>



<main aria-labelledby="title1">
  <Router {routes} />
  
</main>

