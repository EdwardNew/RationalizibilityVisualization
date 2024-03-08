<script>
  import Graph from '../components/Graph.svelte';
  import Scrolleyteller from './Scrolleyteller.svelte';
</script>

<svelte:head>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Madimi+One&display=swap" rel="stylesheet">
</svelte:head>

<main>
  <Scrolleyteller />
</main>

<style>
  :global(body) {
    background-image: linear-gradient(to top, #1e3c72 0%, #1e3c72 1%, #2a5298 100%);
  }

  :global(body) {
    font-family: "Madimi One", sans-serif;
    font-weight: 400;
    font-style: normal;
  }

  /* main {
    display: flex;
    flex-direction: column;
    align-items: center;
  } */


</style>
