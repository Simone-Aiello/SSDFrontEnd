<script lang="ts">
  import Router, {location, link} from 'svelte-spa-router';
  import Home from './components/Home.svelte';
  import Login from './components/Login.svelte';
  import Register from './components/Register.svelte';
  import Reservations from './components/Reservations.svelte';
  import ReservationsForm from './components/ReservationsForm.svelte';
</script>

<main>
  <head>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="">
    <link href="https://fonts.googleapis.com/css2?family=Roboto&display=swap" rel="stylesheet">
  </head>
  <Router routes={{
    '/': Home,
    '/login': Login,
    '/register': Register,
    '/reservations': Reservations,
    '/newreservation': ReservationsForm,
  }} />

</main>

<style>
  main{
    font-family: 'Roboto', sans-serif;
  }
</style>