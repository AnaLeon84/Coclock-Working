<script>
    import {push} from "svelte-spa-router"

    export let reload=false
    let email = "test@gmail.com";
    let pwd = "test";
    
    const handleSubmit=async()=>{
        const token=await login();
        localStorage.setItem("token", token);
        if(reload) location.reload();

        push("/")
  };



  const login = async () => {
    const response = await fetch(import.meta.env.VITE_API_URL + "/auth/login", {
      method: "POST",
      headers: {
        "Content-type": "application/json",
      },
      body: JSON.stringify({
        email,
        password: pwd,
      }),
    });
    const json = await response.json();
    return json.data.access_token;
  };
</script>

<h2 id="title1">Se connecter</h2>

<form on:submit|preventDefault={handleSubmit} aria-label="Informations de connexion">
  <label for="email">Email</label>
  <input
    required
    type="email"
    name="email"
    id="email"
    placeholder="ex : m.dupont@monmail.com"
    bind:value={email}
  />

  <label for="password">Mot de passe</label>
  <input
    required
    type="password"
    name="password"
    id="password"
    placeholder="***********"
    bind:value={pwd}
  />

  <input type="submit" value="Se connecter" />
</form>
