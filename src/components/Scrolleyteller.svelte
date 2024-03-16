<script>
    // import * as d3 from 'd3';
  import Scroller from '@sveltejs/svelte-scroller';
  
  import Title from './Title.svelte';
  import Graph from './Graph.svelte';
  import GraphScroll from './GraphScroll.svelte';
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
    <Title {index}/>
    <progress id="scrollProgress" value={progress || 0} />
    <!-- <div class="progress-bars">
        <p>current section: <strong>{index + 1}/{count}</strong></p>
        <progress value={count ? (index + 1) / count : 0} />
        <p>offset in current section</p>
        <progress value={offset || 0} />
        <p>total progress</p>
        <progress value={progress || 0} />
      </div> -->
  </div>

  <div class="foreground" slot="foreground">
    <Text {index} />
    <GraphScroll {index} />
    <Graph {index} />
    <Graph2 {index} />
    <section class="title"></section>

    <section id="hook"> 
      <div class="hook">
        <p>Ever found yourself caught in the mental web of "I know that they know that I know that they know..."? Surprisingly, there's an entire branch of economics dedicated to formalizing this situation in the decision-making process. At its core, game theory reveals the hidden dynamics governing our everyday choices, shedding light on the strategies of cooperation, competition, and decision-making that defines human interactions. Game theory's reach extends far beyond economics, offering valuable insights into the strategies woven into the fabric of our society.</p>
        <p>Let's walk through an example to see how game theory can be leveraged to optimize decision-making as well as visualize how sometimes there really only is one “correct” option.</p>
      </div>
    </section>

    <section class="transition"></section>

    <section class="introduction"> 
      <div class="intro1">
        <div class="player1">
          <p>Imagine that you are the owner of an ice cream shop...</p>
          <svg 
          class="icecream"
          id="player1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 463.01 998.53"><defs><style>.cls-1{fill:#faa74b;}.cls-2{fill:#f06ca9;}.cls-3{fill:#f5f1ed;}.cls-4{fill:#9bd5c5;}</style></defs><title>2</title><path class="cls-1" d="M690.78,1153.9c-3.37,4.25-4.47,9.45-6,14.47,1.61,3.82.34,6.51-3.26,8.29-13.85,13.57-18.61,13-31.55-3.76-13.44-32.63-22-66.8-32.49-100.41-2.52-8.13.44-9.93,6.88-11.52,17.64-4.35,35.2-9,52.83-13.39,6-1.5,13.25-2.68,11.57-10.83-1.53-7.4-8.79-6.23-14.27-5.39-19.13,2.93-37.79,8-56.13,14-5.91,2-8.21,1.71-10.15-5.18-15.25-54.28-31-108.43-46.74-162.57-1.7-5.83-.41-8.47,5.6-10.82,44.08-17.29,88-34.92,134.92-43.46,5.61-1,13.77-2.42,12-10.66s-9-7.31-15.81-6c-46.07,8.69-89.55,25.43-132.62,43.16-9.58,3.94-11.83,1.66-14.07-7.28-4.69-18.72-10.75-37.09-15.75-55.73-2-7.29-5.69-11.34-13.06-13.71A276.18,276.18,0,0,1,486.79,759c-13.93-6.78-23-18.16-26.73-33.57-3.5-14.61,1.15-25.13,14.74-31.92,13.21-6.61,27.69-8.24,42-8.53,49-1,98,2,147,3.32,61.45,1.64,123,5.13,184.12-5.65,17.32-3.05,27.88,6.33,28.14,23.72.46,29.61-11.74,47.64-39.53,58.39-2.82,1.09-5.65,2.73-8.57,3-20.93,1.67-29.55,14.41-35,33.52-7.83,27.66-17.23,54.9-24.49,82.75-2,7.5-5.29,12.12-13.13,15.07-30.49,11.44-60.61,23.86-91.59,34-6.38,2.09-15.58,5.22-13.15,12.71,2.62,8,12,5.7,18.81,3.21,29.52-10.77,59-21.73,90-33.19-7.36,27.74-14.54,54.62-21.6,81.53C724,1050.11,708.86,1102.45,690.78,1153.9Z" transform="translate(-425.2 -187.69)"/><path class="cls-2" d="M838.1,476.86A77.89,77.89,0,0,0,827,471.14c-5.24-2.25-9-1.3-11.84,4.53-8.4,17.49-23.07,28.8-39.45,38.17-14.21,8.14-29.45,13.8-45.22,18-53.45,14.4-108.82,15-163.19,22.72-30,4.27-60.05,8.45-88.35,20.14-25.11,10.38-43.67,27.13-51.11,54.18-5.14,18.7-3.49,36,10.83,50.65,4.09,4.18,8.36,5.61,14.32,3.85,13.46-4,27.34-6,41.23-8,51.07-7.32,102.38-3.95,148.19-3.91,30.87,1.72,56.34,2.06,81.78,1.09s50.77-2.54,76-6.2c39.1-5.67,70.06-22.09,83.45-62C898.16,561.1,876.74,501.41,838.1,476.86ZM507.38,610.13c-18.55,5.69-34.66,15.1-46.95,30.49-2.06,2.58-4.68,5.74-8.49,4.66s-2.38-5-2.75-7.84C449.53,614,480,586.16,503.29,588c7,.56,12.85,3.3,14.19,10.74S513.11,608.38,507.38,610.13Zm353.41-47.22c-2.22,3.35-4.92,5.63-9.22,5.44s-6.27-3.3-6.92-6.48c-3.52-17-10.42-32.17-23.31-44.24-2-1.88-3.56-4.52-1.73-7.05s4.73-1.58,7.34-1c18.29,3.93,35.41,24.8,35.66,43.5C862.45,556.42,862.83,559.84,860.79,562.91Z" transform="translate(-425.2 -187.69)"/><path class="cls-2" d="M814.07,396.42c-6.26-36.68-25.84-64.22-61-79.19-6.88-2.93-11.19-2.19-15.48,5.28-22.55,39.26-55.94,66.37-97.26,84.1-28.37,12.17-58.27,18.93-88.4,24.85-26,5.1-52.31,8.88-76.53,20.57-26.71,12.89-39,34.72-40,61.77.27,15.42,2.42,28.47,9,40.33,3.65,6.54,8.57,12.54,17.29,8.19,31.74-15.86,66.3-20.81,100.71-26.27,34.17-5.42,69-2.84,103.21-8.79,34.87-6.07,69.49-13.5,99.92-32.38C801.75,472.41,821.58,440.36,814.07,396.42ZM510.42,479c-18.33,5.41-33.79,15-45.93,29.84-2.1,2.57-4.45,5.49-8.48,4.61-2.42-.53-2.92-2.63-2.59-8.91,1.24-23,29.68-49,52.71-48.24,7.67.25,13.82,3.21,15.37,10.81C523.11,475,516.17,477.25,510.42,479ZM782,389.76c-5.2,1.22-5.73-3.34-6.66-7-2-7.81-5.15-15-10.94-20.82-1.8-1.8-4.53-3.78-2.3-6.62,1.79-2.26,4.2-.22,6.2.49C778.6,359.43,787,370.69,787,380.65,787.4,384.82,786.23,388.77,782,389.76Z" transform="translate(-425.2 -187.69)"/><path class="cls-2" d="M676,188.39c-18.21-2.63-34.9,1.9-47.13,16.7-10,12.07-7.41,29.28,4.78,35.48,4.58,2.33,9.36,4.31,10.47-4,.8-6,5-9.45,10.76-10.12,6.3-.72,10.89,2.67,14.07,7.85a23.67,23.67,0,0,1,2.83,19.61c-5.19,18.72-19,29.6-34.91,38.46-18.57,10.35-38.63,17.35-58.26,25.33-21.81,8.87-43.13,18.68-61.61,33.69-16,13-28.38,28.52-32.37,49.31-2.71,14.08,7.33,27.54,18,24.52,23-6.5,46.63-10,70.06-14.43,58.48-11,107.8-37.32,144-85.72,14.37-19.21,20.41-41.13,21.11-62.41C736.78,222.74,712.23,193.63,676,188.39ZM551,374.78c-13.24,4.25-25,10.8-33.83,22-1.62,2-3.82,4.27-6.78,2.88-3.14-1.47-1.92-4.68-1.61-7.08,2.13-16.37,22.09-34.2,38-34.17,5.44-.2,10.09,1.57,11.63,7.06C560.06,371.28,555.71,373.27,551,374.78Z" transform="translate(-425.2 -187.69)"/><path class="cls-3" d="M658.09,206.64c13.68-9,30.18-6.43,37.9,4.28,2.21,3.07,3.29,6.43,1,9.3-2.75,3.45-6.18,1.71-8.83-.77C680.13,211.93,669.65,209.91,658.09,206.64Z" transform="translate(-425.2 -187.69)"/></svg>
        </div>
        <div class="player2">
          <p>...And you find yourself in a competitive situation with a new ice cream shop that has opened just across the street.</p>
          <svg 
          class="icecream"
          id="player2" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 463.01 998.53"><defs><style>.cls-1{fill:#faa74b;}.cls-2{fill:#f06ca9;}.cls-3{fill:#f5f1ed;}.cls-4{fill:#9bd5c5;}</style></defs><title>2</title><path class="cls-1" d="M690.78,1153.9c-3.37,4.25-4.47,9.45-6,14.47,1.61,3.82.34,6.51-3.26,8.29-13.85,13.57-18.61,13-31.55-3.76-13.44-32.63-22-66.8-32.49-100.41-2.52-8.13.44-9.93,6.88-11.52,17.64-4.35,35.2-9,52.83-13.39,6-1.5,13.25-2.68,11.57-10.83-1.53-7.4-8.79-6.23-14.27-5.39-19.13,2.93-37.79,8-56.13,14-5.91,2-8.21,1.71-10.15-5.18-15.25-54.28-31-108.43-46.74-162.57-1.7-5.83-.41-8.47,5.6-10.82,44.08-17.29,88-34.92,134.92-43.46,5.61-1,13.77-2.42,12-10.66s-9-7.31-15.81-6c-46.07,8.69-89.55,25.43-132.62,43.16-9.58,3.94-11.83,1.66-14.07-7.28-4.69-18.72-10.75-37.09-15.75-55.73-2-7.29-5.69-11.34-13.06-13.71A276.18,276.18,0,0,1,486.79,759c-13.93-6.78-23-18.16-26.73-33.57-3.5-14.61,1.15-25.13,14.74-31.92,13.21-6.61,27.69-8.24,42-8.53,49-1,98,2,147,3.32,61.45,1.64,123,5.13,184.12-5.65,17.32-3.05,27.88,6.33,28.14,23.72.46,29.61-11.74,47.64-39.53,58.39-2.82,1.09-5.65,2.73-8.57,3-20.93,1.67-29.55,14.41-35,33.52-7.83,27.66-17.23,54.9-24.49,82.75-2,7.5-5.29,12.12-13.13,15.07-30.49,11.44-60.61,23.86-91.59,34-6.38,2.09-15.58,5.22-13.15,12.71,2.62,8,12,5.7,18.81,3.21,29.52-10.77,59-21.73,90-33.19-7.36,27.74-14.54,54.62-21.6,81.53C724,1050.11,708.86,1102.45,690.78,1153.9Z" transform="translate(-425.2 -187.69)"/><path class="cls-4" d="M838.1,476.86A77.89,77.89,0,0,0,827,471.14c-5.24-2.25-9-1.3-11.84,4.53-8.4,17.49-23.07,28.8-39.45,38.17-14.21,8.14-29.45,13.8-45.22,18-53.45,14.4-108.82,15-163.19,22.72-30,4.27-60.05,8.45-88.35,20.14-25.11,10.38-43.67,27.13-51.11,54.18-5.14,18.7-3.49,36,10.83,50.65,4.09,4.18,8.36,5.61,14.32,3.85,13.46-4,27.34-6,41.23-8,51.07-7.32,102.38-3.95,148.19-3.91,30.87,1.72,56.34,2.06,81.78,1.09s50.77-2.54,76-6.2c39.1-5.67,70.06-22.09,83.45-62C898.16,561.1,876.74,501.41,838.1,476.86ZM507.38,610.13c-18.55,5.69-34.66,15.1-46.95,30.49-2.06,2.58-4.68,5.74-8.49,4.66s-2.38-5-2.75-7.84C449.53,614,480,586.16,503.29,588c7,.56,12.85,3.3,14.19,10.74S513.11,608.38,507.38,610.13Zm353.41-47.22c-2.22,3.35-4.92,5.63-9.22,5.44s-6.27-3.3-6.92-6.48c-3.52-17-10.42-32.17-23.31-44.24-2-1.88-3.56-4.52-1.73-7.05s4.73-1.58,7.34-1c18.29,3.93,35.41,24.8,35.66,43.5C862.45,556.42,862.83,559.84,860.79,562.91Z" transform="translate(-425.2 -187.69)"/><path class="cls-4" d="M814.07,396.42c-6.26-36.68-25.84-64.22-61-79.19-6.88-2.93-11.19-2.19-15.48,5.28-22.55,39.26-55.94,66.37-97.26,84.1-28.37,12.17-58.27,18.93-88.4,24.85-26,5.1-52.31,8.88-76.53,20.57-26.71,12.89-39,34.72-40,61.77.27,15.42,2.42,28.47,9,40.33,3.65,6.54,8.57,12.54,17.29,8.19,31.74-15.86,66.3-20.81,100.71-26.27,34.17-5.42,69-2.84,103.21-8.79,34.87-6.07,69.49-13.5,99.92-32.38C801.75,472.41,821.58,440.36,814.07,396.42ZM510.42,479c-18.33,5.41-33.79,15-45.93,29.84-2.1,2.57-4.45,5.49-8.48,4.61-2.42-.53-2.92-2.63-2.59-8.91,1.24-23,29.68-49,52.71-48.24,7.67.25,13.82,3.21,15.37,10.81C523.11,475,516.17,477.25,510.42,479ZM782,389.76c-5.2,1.22-5.73-3.34-6.66-7-2-7.81-5.15-15-10.94-20.82-1.8-1.8-4.53-3.78-2.3-6.62,1.79-2.26,4.2-.22,6.2.49C778.6,359.43,787,370.69,787,380.65,787.4,384.82,786.23,388.77,782,389.76Z" transform="translate(-425.2 -187.69)"/><path class="cls-4" d="M676,188.39c-18.21-2.63-34.9,1.9-47.13,16.7-10,12.07-7.41,29.28,4.78,35.48,4.58,2.33,9.36,4.31,10.47-4,.8-6,5-9.45,10.76-10.12,6.3-.72,10.89,2.67,14.07,7.85a23.67,23.67,0,0,1,2.83,19.61c-5.19,18.72-19,29.6-34.91,38.46-18.57,10.35-38.63,17.35-58.26,25.33-21.81,8.87-43.13,18.68-61.61,33.69-16,13-28.38,28.52-32.37,49.31-2.71,14.08,7.33,27.54,18,24.52,23-6.5,46.63-10,70.06-14.43,58.48-11,107.8-37.32,144-85.72,14.37-19.21,20.41-41.13,21.11-62.41C736.78,222.74,712.23,193.63,676,188.39ZM551,374.78c-13.24,4.25-25,10.8-33.83,22-1.62,2-3.82,4.27-6.78,2.88-3.14-1.47-1.92-4.68-1.61-7.08,2.13-16.37,22.09-34.2,38-34.17,5.44-.2,10.09,1.57,11.63,7.06C560.06,371.28,555.71,373.27,551,374.78Z" transform="translate(-425.2 -187.69)"/><path class="cls-3" d="M658.09,206.64c13.68-9,30.18-6.43,37.9,4.28,2.21,3.07,3.29,6.43,1,9.3-2.75,3.45-6.18,1.71-8.83-.77C680.13,211.93,669.65,209.91,658.09,206.64Z" transform="translate(-425.2 -187.69)"/></svg>
        </div>
      </div>

      <div class="intro2">
        <p>Every day before opening, both <mark id="player1">you</mark> and <mark id="player2">your rival</mark> must decide on the price at which you will sell your ice cream for the day. Each player’s decision is unknown to the other until opening time. This is called a <b>simultaneous game</b>. </p>
        <p id="focus">The choices are simple: a higher price (H) or a lower price (L).</p>
        <p>As a rational and strategic individual, you base your decisions on what you anticipate your rival might do, with the ultimate goal of maximizing your own profits by the end of the day.</p>
      </div>
    </section>

    <section class="graph1">
      <!-- <div class="graph1">
        <p>In this game of ice cream pricing, the concept of dominance plays a pivotal role in shaping your decisions and responses.</p>
        <p>A strategy is <b>dominant</b> if it outperforms all other available strategies, regardless of the choices made by your rival.</p>
        <p>We will focus on two strategies, setting a high price, or setting a low price. Scroll on to see the different types of dominance that can take place.</p>
      </div> -->
      <!-- <div class="graph1">
        <p>Consider the scenario where one pricing strategy consistently yields a higher profit than the other, irrespective of your rival's choice. In this case, that strategy is said to be <b>strictly dominant</b>. </p>
        <p>If setting a higher price consistently results in more profit, it is a dominant strategy for you. The dominance concept simplifies decision-making by allowing you to focus on the strategy that consistently performs better.</p>
      </div>
      <div class="graph1">
        <p>Now consider a scenario in which one pricing strategy yields a higher payoff than the other in some cases, and at lease as high a payoff in all other cases, regardless of the choices made by your rival. In this case, the strategy is said to be <b>weakly dominant</b>.</p>
        <p>If setting a higher price consistently results in more profit, it is a dominant strategy for you. The dominance concept simplifies decision-making by allowing you to focus on the strategy that consistently performs better.</p>
      </div> -->
    </section>

    <section class="graph1"></section>

    <section class="graph1"></section>

    <section class="graph1"></section>

    <section>
      <div class="graph2">
        <p>
          Now imagine that rather than choosing only between a higher or lower price, you can choose exactly how much your ice cream costs.
        </p>
        <p>
            These days the industry standard price for a scoop of ice cream is somewhere between [1 and 4] dollars.
        </p>
      </div>
    </section>

    <section>section 7</section>

    <section>section 8 takeaway</section>

  </div>

</Scroller>

<style>
    #scrollProgress {
        width: 100%;
        border: none;
        margin: 0;
        padding: 0;
        position: fixed;
        top: 0;
        left: 0;
        height: 4px;
        background-color: white !important;
    }
    .background {
      width: 70%;
      height: 70vh;
      position: relative;
    }
  
    .foreground {
      width: 70%;
      margin: 0 auto;
      height: auto;
      position: relative;
    }
  
    /* .progress-bars {
      position: absolute;
      background: rgba(170, 51, 120, 0.2);
      visibility: visible;
    } */

    .title {
      height: 50vh;
    }

    .hook {
      width: 85%;
      transform: translateY(400px);
      margin: 0 auto;
      padding: 20px;
      border: 2px solid;
      border-color: #5585b5;
      border-radius: 10px;
      box-shadow: 60px -16px #5585b5;
      background-color: rgba(255, 255, 255);
    }

    div.hook p {
      line-height: 1.5;
      text-align: center;
      padding: 15px;
      padding-left: 20px;
      padding-right: 20px;
      color: "white";
      font-size: larger;
    }

    .transition {
      height: 60vh;
      text-align: center;
    }

    .introduction {
      height: 180vh;
    }
  
    .intro1 div.player1 {
      width: 40%;
      padding: 20px;
      transform: translate(25px, 250px);
      border: 2px solid;
      border-color: #e35b99;
      border-radius: 10px;
      box-shadow: 60px -16px #e35b99;
      background-color: rgba(255, 255, 255);
      font-size: larger;
    }

    .intro1 div.player2 {
      width: 40%;
      padding: 20px;
      transform: translate(500px, 350px);
      border: 2px solid;
      border-color: #49c4a2;
      border-radius: 10px;
      box-shadow: 60px -16px #49c4a2;
      background-color: rgba(255, 255, 255);
      font-size: larger;
    }

    .icecream {
      width: 50px;
    }

    .intro2 {
      width: 85%;
      transform: translateY(500px);
      /* margin: 0 auto; */
      padding: 30px;
      font-size: larger;
      border: 2px solid;
      border-color: #5585b5;
      border-radius: 10px;
      box-shadow: 60px -16px #5585b5;
      background-color: rgba(255, 255, 255);
    }

    mark#player1{
      font-weight: bold;
      background-color: transparent;
      color: #e35b99;
    }

    mark#player2{
      font-weight: bold;
      background-color: transparent;
      color: #49c4a2;
    }
    
    .intro2 #focus{
      color:#5585b5;
      font-size: larger;    
      font-weight: bold;  
    }

    section.graph1 {
      height: 80vh;
    }

    /* div.graph1 {
      width: 30%;
      transform: translate(-150px, 10px);
      margin: 0 auto;
      padding: 20px;
      font-size: larger;
      border: 2px solid;
      border-color: #5585b5;
      border-radius: 10px;
      box-shadow: 20px -16px #5585b5;
      background-color: rgba(255, 255, 255);
      margin-bottom: 50px;
    } */

    .graph2 {
      width: 75%;
      transform: translate(50px, 350px);
      padding: 30px;
      font-size: larger;
      border: 2px solid;
      border-color: #5585b5;
      box-shadow: 40px -16px #5585b5;
      background-color: rgba(255, 255, 255);
    }

    section {
      height: 120vh;
      /* background-color: rgba(0, 0, 0, 0.2); */
      background-color: transparent;
      text-align: center;
      max-width: 1000px; /* adjust at will */
      color: black;
      padding: 1em;
      margin: 0 0 2em 0;
    }

  </style>
