<script>
    import logo from '../assets/coclockworking.png';
    
    import { link } from "svelte-spa-router";
    const nav_links = [
      { label: "Page d'accueil du site", text: "Accueil", url: "/" },
      {
        label: "Page de reservation de salles",
        text: "Réservation",
        url: "/espaces",
      },
      { label: "Formulaire de connexion", text: "Se connecter", url: "/login" },
      {
        label: "Formulaire d'inscription au site",
        text: "S'enregistrer",
        url: "/signin",
      },
    ];
  </script>
  
  <nav aria-label="header navigation">
    <a
      href="/"
      class="logo"
      title="Co'Clock Working"
      aria-label="accueil du site"
      use:link
    >
      <img src="./coclockworking.png" alt="Logo de l'entreprise" />
    </a>
  
    <ul id="menu2" class="links" role="menu" aria-labelledby="menubutton">
      {#each nav_links as nav}
        <li role="none">
          <a role="menuitem" aria-label={nav.label} href={nav.url} use:link
            >{nav.text}</a
          >
        </li>
      {/each}
    </ul>
  
    <button
      type="button"
      id="menubutton"
      aria-haspopup="true"
      aria-controls="menu2"
    >
      <i class="fa-solid fa-bars" />
    </button>
  </nav>
  