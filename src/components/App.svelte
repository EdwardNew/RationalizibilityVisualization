<script>
  import Graph from '../components/Graph.svelte';
  import Scrolleyteller from './Scrolleyteller.svelte';
</script>

<svelte:head>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Madimi+One&display=swap" rel="stylesheet">
    <link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Raleway:ital,wght@0,100..900;1,100..900&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap" rel="stylesheet">
</svelte:head>

<main>
  <Scrolleyteller />
</main>

<style>
  :global(body) {
    background-image: linear-gradient(to top, #1e3c72 0%, #1e3c72 1%, #2a5298 100%);
    margin: 0;
    padding: 0;
    color:white;
  }

  :global(body) {
  font-family: "Roboto", sans-serif;
  font-weight: 400;
  font-style: normal;
}


  :global(h1) {
    font-family: "Madimi One", sans-serif;
    font-weight: 400;
    font-style: normal;
  }

</style>
