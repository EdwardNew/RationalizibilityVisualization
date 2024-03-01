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

    let dominanceText = "Alternative B strictly dominates R."

    let xScale = d3.scaleLinear([0, 5], [0, width]);
    const xAxis = [...Array(5).keys()];
    
    let yScale = d3.scaleLinear([0, 5], [0, height]);
    const yAxis = [...Array(5).keys()];

    let player1 = {'x':xScale(2), 'y':yScale(2), 'color': 'red'};
    let player2 = {'x':xScale(3), 'y':yScale(3), 'color': 'blue'};
    let players = [player1, player2];
    let pmap = {'player1': player1, 'player2': player2}
    

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

    function dragstarted(d) {
        console.log('start');
        d3.select(this)
          .attr('stroke',"black")
          .attr('stroke-width',"5");
    }

    function dragmove(d) {
        console.log('moving');
        d3.select(this)
            .attr("cx", d.x)
            .attr("cy", d.y);
    }

    function dragend(d) {
        console.log('end');
        d3.select(this)
          .attr('stroke',"none");

        pmap[d3.select(this).attr("id")].x = d.x;
        pmap[d3.select(this).attr("id")].y = d.y;

       console.log(players);
       console.log(pmap["player2"].x, pmap["player2"].y);
       console.log(pmap["player1"].x, pmap["player1"].y);

       if (pmap["player1"].x > pmap["player2"].x && pmap["player1"].y < pmap["player2"].y) {
            console.log("Alternative R strictly dominates B.");
            dominanceText = "Alternative R strictly dominates B.";
        } else if (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) {
            console.log("Alternative B strictly dominates R.");
            dominanceText = "Alternative B strictly dominates R.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) || (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative B weakly dominates R.");
            dominanceText = "Alternative B weakly dominates R.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y < pmap["player2"].y) || (pmap["player1"].x > pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative R weakly dominates B.");
            dominanceText = "Alternative R weakly dominates B.";
        } else {
            console.log("No dominance relationship.");
            dominanceText = "No dominance relationship.";
        }
    }
</script>

<h1>Rationalizibility</h1>
<p>Visualizing strict/weak dominance</p>

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
            cx={d.x}
            cy={d.y}
            fill={d.color}
            r="10"
            />  
        {/each}
    </g>
</svg>

<div>
    <p>{dominanceText}</p>
</div>


<style>
svg {
    max-width: 100%;
    height: auto;
  }
</style>