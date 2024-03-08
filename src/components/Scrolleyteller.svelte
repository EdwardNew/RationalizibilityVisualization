<script>
    // import * as d3 from 'd3';
    import Scroller from '@sveltejs/svelte-scroller';
    
    import Title from './Title.svelte';
    import Graph from './Graph.svelte';
    import Graph2 from './Graph2.svelte';
    import Intro from './Intro.svelte';
    import Text from './Text.svelte';
    
  
    let count, index, offset, progress;

</script>

<Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background">
    <Title {index} />
    <div class="progress-bars">
        <p>current section: <strong>{index + 1}/{count}</strong></p>
        <progress value={count ? (index + 1) / count : 0} />
        <p>offset in current section</p>
        <progress value={offset || 0} />
        <p>total progress</p>
        <progress value={progress || 0} />
      </div>
  </div>

  <div class="foreground" slot="foreground">
    <Text {index} />
    <Graph {index} />
    <Graph2 {index} />
    <section>This is the first section. </section>
    <section>This is the second section.</section>
    <section>This is the third section.</section>
    <section>This is the fourth section.</section>
    <section>This is the fifth section.</section>

  </div>

</Scroller>

<style>
    .background {
      width: 70%;
      height: 70vh;
      position: relative;
    }
  
    .foreground {
      width: 50%;
      margin: 0 auto;
      height: auto;
      position: relative;
    }
  
    .progress-bars {
      position: absolute;
      background: rgba(170, 51, 120, 0.2);
      visibility: visible;
    }
  
    section {
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.2);
      background-color: transparent;
      text-align: center;
      max-width: 1000px; /* adjust at will */
      color: black;
      padding: 1em;
      margin: 0 0 2em 0;
    }
  </style>
