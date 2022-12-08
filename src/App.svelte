<script lang="ts">
  import Router, {location, link} from 'svelte-spa-router';
    import Home from './components/Home.svelte';
  import Login from './components/Login.svelte';
    import Register from './components/Register.svelte';
  import Reservations from './components/Reservations.svelte';
</script>

<main>
  <Router routes={{
    '/': Home,
    '/login': Login,
    '/register': Register,
    '/reservations': Reservations
  }} />

</main>

<style>

</style>