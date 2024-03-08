<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import { fly, fade, draw } from "svelte/transition";
    import { tweened } from "svelte/motion";
    import { cubicOut, cubicInOut, cubicIn } from "svelte/easing";

    const width = 500;
    const height = 500;
    const marginTop = 50;
    const marginRight = 50;
    const marginBottom = -50;
    const marginLeft = 0;
    const axisBound = 6;

    let dominanceText = "No dominance relationship."

    let xScale = d3.scaleLinear([0, axisBound], [0, width]);
    const xAxis = [...Array(axisBound).keys()];
    
    let yScale = d3.scaleLinear([0, axisBound], [0, height]);
    const yAxis = [...Array(axisBound).keys()];

    var gx = d3.scaleLinear()
        .domain([0, axisBound])
        .range([0, width]);

    var gy = d3.scaleLinear()
        .domain([0, axisBound])
        .range([height, 0]);


    let player1 = {'x':xScale(2), 'y':yScale(2), 'color': 'red'};
    let player2 = {'x':xScale(3), 'y':yScale(3), 'color': 'blue'};
    let players = [player1, player2];
    let pmap = {'player1': player1, 'player2': player2}
    

    let mousePosition = [0, 0];
    function recordMousePosition(event) {
        mousePosition = d3.pointer(event);
    }

    let mounted = false;

    onMount(() => {
        mounted = true;

        d3.select("#chart2")
            .append("g")
            .attr("transform", "translate(0, 500)")
            .call(d3.axisBottom(gx));

        d3.select("#chart2")
            .append("g")
            .attr("transform", "translate(0, 0)") 
            .call(d3.axisLeft(gy));
    });

    function dragstarted(d) {
        console.log('start');
        d3.select(this)
          .attr('stroke',"black")
          .attr('stroke-width',"5");
    }

    function dragmove(d) {
        console.log('moving');

        const [snapX, snapY] = snapToGrid([d.x, d.y]);

        const maxX = width - marginLeft - marginRight;
        const maxY = height - marginTop - marginBottom;

        const clampedX = Math.max(Math.min(snapX, maxX), marginLeft);
        const clampedY = Math.max(Math.min(snapY, maxY), marginTop);


        d3.select(this)
            .attr("cx", clampedX)
            .attr("cy", clampedY);
    }

    function dragend(d) {
        console.log('end');
        d3.select(this)
          .attr('stroke',"none");

        const [snapX, snapY] = snapToGrid([d.x, d.y]);

        const maxX = width - marginLeft - marginRight;
        const maxY = height - marginTop - marginBottom;

        const clampedX = Math.max(Math.min(snapX, maxX), marginLeft);
        const clampedY = Math.max(Math.min(snapY, maxY), marginTop);

        pmap[d3.select(this).attr("id")].x = clampedX;
        pmap[d3.select(this).attr("id")].y = clampedY;

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

    function snapToGrid([x, y]) {
        // Define grid interval
        const gridSize = 50;

        // Snap to the nearest grid point
        const snapX = Math.round(x / gridSize) * gridSize;
        const snapY = Math.round(y / gridSize) * gridSize;

        return [snapX, snapY];
    }

    export let index;
    let isVisible= false;

    $: if (index === 3) {
        isVisible = true;
    } else {
        isVisible = false;
    }



    // New Curve Plotting Code

    function equation1(x) {
        return 8/(x+2)
    }

    function equation2(x) {
        return (8/x) - 2;
    }

    const br1 = d3.line()
    .x((d) => xScale(d))
    .y((d) => height - yScale(equation1(d)));

    const br2 = d3.line()
    .x((d) => xScale(d))
    .y((d) => height - yScale(equation2(d)));

    const data = d3.range(0, axisBound, 0.1);
    const inverseData = d3.range(axisBound, 0, -0.1);


    let iteration = 0;
    let currentBounds = [0,6];


    function iterate() {
        iteration++;
        console.log('interate', iteration);
        currentBounds = calcBounds(currentBounds);
        console.log(iteration, currentBounds);
        document.querySelector('#RSet').innerHTML += `<p>[${currentBounds[0].toFixed(4)}, ${currentBounds[1].toFixed(4)}] x [${currentBounds[0].toFixed(4)}, ${currentBounds[1].toFixed(4)}]</p>`
    }

    function back() {
        if (iteration > 0) {
            iteration--;
        }

        console.log('back', iteration)
    }

    function calcBounds(currentBounds) {
        return [calcBound(currentBounds[1]), calcBound(currentBounds[0])];
    }

    function calcBound(bound) {
        return equation1(bound);
    }

    function undoBounds(currentBounds) {
        return [calcBound(currentBounds[1]), calcBound(currentBounds[0])];
    }

    function undoBound(bound) {
        return equation1(bound);
    }


</script>
<div id='graph' class:show={isVisible}>

<svg {width} {height} viewBox="0 -5 {width-65} {height+65}" on:pointermove={recordMousePosition} id="chart2">
    <!-- axis -->
    <g stroke="lightgray" stroke-width="0.5" transition:fade={{ delay: 250, duration: 5000 }}>
        {#each xAxis as x}
            {#if isVisible}
                <line x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(x)} 0)' stroke='black' stroke-width='1' in:draw={{ duration: 1000 }}></line>
            {/if}
        {/each}

        {#each yAxis as y}
            {#if isVisible}
                <line x1="0" y1="{yScale(1)}" x2="{width}" y2="{yScale(1)}" transform='translate(0 {yScale(y)})' stroke='black' stroke-width='1' in:draw={{ duration: 1000 }}></line>
            {/if}
        {/each}
    </g>

    {#if isVisible}
        <path d={br1(data)} stroke='red' fill='none' stroke-width='5' in:draw={{ duration: 2000, delay: 1000 }}></path>
        <path d={br2(inverseData)} stroke='blue' fill='none' stroke-width='5' in:draw={{ duration: 2000, delay: 1000 }}></path>
    {/if}

  
        {#if iteration}
            <line class="line" x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(currentBounds[0])} 0)' stroke='green' stroke-width='5' in:draw={{ duration: 1000 }}></line>
            <line class="line" x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(currentBounds[1])} 0)' stroke='green' stroke-width='5' in:draw={{ duration: 1000 }}></line>

            <line class="line" x1="0" y1="0" x2="{width}" y2="0" transform='translate(0 {height - yScale(currentBounds[0])})' stroke='green' stroke-width='5' in:draw={{ duration: 1000, delay: 1000 }}></line>
            <line class="line" x1="0" y1="0" x2="{width}" y2="0" transform='translate(0 {height - yScale(currentBounds[1])})' stroke='green' stroke-width='5' in:draw={{ duration: 1000, delay: 1000 }}></line>
        {/if}
   

    <text
        class="xlabel"
        text-anchor="middle"
        x={width-250}
        y={height+40}
    >
        Your Chosen Price of Ice Cream
    </text>

    <text
        class="ylabel"
        text-anchor="middle"
        transform="rotate(-90)"
        x={-250}
        y={-40}
    >
        Rival's Chosen Price of Ice Cream
    </text>
</svg>

<button on:click={back}>&lt;</button>
<button on:click={iterate}>&gt;</button>

<div id="RSet">
    <p>Rationalizable Set</p>
    <p>[&infin;,&infin;] x [&infin;,&infin;]</p>
</div>



</div>

<style>
#graph {
    position:fixed;
    display: none;
}

.line {
    transition: all 1s;
}

#graph.show {
    display: block;
}

svg {
    max-width: 100%;
    height: auto;
  }
</style>
