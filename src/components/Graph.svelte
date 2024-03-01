<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import { circIn } from 'svelte/easing';

    const width = 928;
    const height = 500;
    const marginTop = 10;
    const marginRight = 10;
    const marginBottom = 20;
    const marginLeft = 40;

    let xScale = d3.scaleLinear([0, 5], [0, width]);
    const xAxis = [...Array(5).keys()];
    
    let yScale = d3.scaleLinear([0, 5], [0, height]);
    const yAxis = [...Array(5).keys()];

    let player1 = {'x':2, 'y':2, 'color': 'red'};
    let player2 = {'x':3, 'y':3, 'color': 'blue'};
    let players = [player1, player2];
    

    let mousePosition = [0, 0];
    function recordMousePosition(event) {
        mousePosition = d3.pointer(event);
    }

    onMount(() => {
        d3.selectAll('circle').call(d3.drag()
          .on('start', dragstarted)
          .on('drag', dragmove)
          .on('end', dragend));
        console.log(d3.selectAll('circle'));
    });

    function dragstarted() {
        console.log('start');
    }

    function dragmove(d) {
        console.log('moving!');
    }

    function dragend() {
        console.log('end');
    }


</script>

<h1>Rationalizibility</h1>

<svg {width} {height} viewBox="0 0 {width} {height}" on:pointermove={recordMousePosition}>
    
    <!-- axis -->
    <g stroke="lightgray" stroke-width="0.5">
        {#each xAxis as x}
            <line x="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(x)} 0)' stroke='black' stroke-width='1'></line>
        {/each}

        {#each yAxis as y}
            <line x="0" y1="{yScale(1)}" x2="{width}" y2="{yScale(1)}" transform='translate(0 {yScale(y)})' stroke='black' stroke-width='1'></line>
        {/each}
    </g>

    <!-- points -->
    <g stroke="#000" stroke-opacity="0.2">
        {#each players as d, i}
            <circle
            key={i}
            id='player{i+1}'
            cx={xScale(d.x)}
            cy={yScale(d.y)}
            fill={d.color}
            r="10"
            />  
        {/each}
    </g>
</svg>


<style>
svg {
    max-width: 100%;
    height: auto;
  }
</style>