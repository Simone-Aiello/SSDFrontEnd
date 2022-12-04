<script lang="ts">
  import Router, {location, link} from 'svelte-spa-router';
  import Login from './components/Login.svelte';
  import Reservations from './components/Reservations.svelte';
</script>

<main>
  <Router routes={{
    '/': Reservations,
    '/login': Login,
  }} />

</main>

<style>

</style>