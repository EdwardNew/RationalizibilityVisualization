<script>
  import Title from '../components/Title.svelte';
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
    background-image: linear-gradient(-225deg, #CBBACC 0%, #2580B3 100%);
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
