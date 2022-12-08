<script lang="ts">
    import { onMount } from 'svelte';
    import axios from 'axios';
    import { push } from 'svelte-spa-router';
    let user = null;
    const getCookieValue = (name: string) => (
      document.cookie.match('(^|;)\\s*' + name + '\\s*=\\s*([^;]+)')?.pop() || ''
    )
    onMount(async () =>{
      await axios("http://localhost:8000/api/v1/auth/user/", {
            withCredentials: true,
            method: "GET",
        }).then((data) =>{
            user = data.data.username;
        })
        .catch((error) =>{
            user = null
        })
    });

    async function doLogout() {
      await axios.post('http://localhost:8000/api/v1/auth/logout/', {
      }, {
        headers: {
          'X-CSRFToken': getCookieValue('csrftoken'),
        },
        withCredentials: true,
      }).then(() =>{
        user = null
        push("/login")
      }).catch((error) =>{
        console.log("Error in do logout: " + error)
      })
    }
</script>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#/">Umbrella reservations</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
            {#if user == null}
              <a class="nav-link active" aria-current="page" href="/#/login">Login</a>
              <a class="nav-link active" aria-current="page" href="/#/register">Register</a>
            {:else}
            <a class="nav-link active" href="/#/reservations">Reservations</a>
              <a class="nav-link active" aria-current="page" href = "/#/" on:click={doLogout}>Logout</a>
            {/if}  
        </div>
      </div>
    </div>
</nav>