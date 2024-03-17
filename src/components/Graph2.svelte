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

    export let index;
    let isVisible= false;

    $: if (index === 10) {
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

    function equation3(x) {
        return (x-2) * 8;
    }

    const br1 = d3.line()
    .x((d) => xScale(d))
    .y((d) => height - yScale(equation1(d)));

    const br2 = d3.line()
    .x((d) => xScale(d))
    .y((d) => height - yScale(equation2(d)));

    const data = d3.range(0, axisBound, 0.1);
    const inverseData = d3.range(4, 0.9, -0.1);


    let iteration = 0;
    let bounds = [[0,6]];
    let currentBounds = [0,6];

    function iterate() {
        iteration++;
        bounds.push(currentBounds);
        currentBounds = calcBounds(currentBounds);
        document.querySelector('#RSet').innerHTML += `<p>[${currentBounds[0].toFixed(4)}, ${currentBounds[1].toFixed(4)}] x [${currentBounds[0].toFixed(4)}, ${currentBounds[1].toFixed(4)}]</p>`
    }

    function back() {
        if (iteration > 0) {
            currentBounds = bounds.pop();
            document.querySelector('#RSet').removeChild(document.querySelector('#RSet').lastChild);
            iteration--;
        }
    }

    function calcBounds(currentBounds) {
        return [calcBound(currentBounds[1]), calcBound(currentBounds[0])];
    }

    function calcBound(bound) {
        return equation1(bound);
    }

</script>
<div id='graph2' class:show={isVisible}>
    <!-- <h3 style="color:white">Visualizing the Rationalizable Set of Actions</h3>
    <p style="color:white">
        These are you and your rival's best response curves. Begin the animation by clicking the right arrow below.<br>
        The pink bound lines represent the rational actions that your rival believes you could take
    </p>
    <p style="color:white">
        Click the arrows at the bottom to iterate through each rationalizable set.
    </p> -->
    <div id='graphContainer'>
        <div style="display: flex;">

        
        <div style="display: flex; flex-direction:column">
            <svg {width} {height} viewBox="40 -40 {width-120} {height+120}" id="chart2">
                <!-- axis -->
                <g stroke="#ffebeb" stroke-width="0.5" transition:fade={{ delay: 250, duration: 5000 }}>
                    {#each xAxis as x}
                        {#if isVisible}
                            <line x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(x)} 0)' stroke='#d3d3d3' stroke-width='1' in:draw={{ duration: 1000 }}></line>
                        {/if}
                    {/each}

                    {#each yAxis as y}
                        {#if isVisible}
                            <line x1="0" y1="{yScale(1)}" x2="{width}" y2="{yScale(1)}" transform='translate(0 {yScale(y)})' stroke='#d3d3d3' stroke-width='1' in:draw={{ duration: 1000 }}></line>
                        {/if}
                    {/each}
                </g>

                {#if isVisible}
                    <path d={br1(data)} stroke='#f06ca9' fill='none' stroke-width='5' in:draw={{ duration: 1000, delay: 250 }}></path>
                    <path d={br2(inverseData)} stroke='#27c297' fill='none' stroke-width='5' in:draw={{ duration: 1000, delay: 250 }}></path>
                {/if}

            
                    {#if iteration}
                        <line class="line" x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(currentBounds[0])} 0)' stroke='#f06ca9' stroke-opacity='0.3' stroke-width='5' in:draw={{ duration: 1000 }}></line>
                        <line class="line" x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(currentBounds[1])} 0)' stroke='#f06ca9' stroke-opacity='0.3' stroke-width='5' in:draw={{ duration: 1000 }}></line>

                        <line class="line" x1="0" y1="0" x2="{width}" y2="0" transform='translate(0 {height - yScale(currentBounds[0])})' stroke='#27c297' stroke-opacity='0.3' stroke-width='5' in:draw={{ duration: 1000, delay: 1000 }}></line>
                        <line class="line" x1="0" y1="0" x2="{width}" y2="0" transform='translate(0 {height - yScale(currentBounds[1])})' stroke='#27c297' stroke-opacity='0.3'stroke-width='5' in:draw={{ duration: 1000, delay: 1000 }}></line>
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

            <div style="display: flex">
                <button style="width: 50%" on:click={back}>&lt;</button>
                <button style="width: 50%" on:click={iterate}>&gt;</button>
            </div>
        </div>

        <div style="overflow-y:auto; height: 500px; padding-right: 1rem">
            <div id="RSet" style="margin-left: 10vw;">
                <p>Rationalizable Set</p>
                <p>[&infin;,&infin;] x [&infin;,&infin;]</p>
            </div>
        </div>

    </div>

    </div>
    <div style="width: 60vw; padding-top: 30px;">
        Click the arrows below the graph to iterate through this domain limiting process. If both players simultaneously iteratively limit the domains of their best response curves based on the other player’s curve, we find that they will eventually converge to a single price for both players. This is the optimal price for both you and your rival to price your ice cream at.
    </div>
</div>

<style>

#graph2 {
    position:fixed;
    display: none;
    margin-top: 10vh;
    color: black;
}

#graphContainer {
    display: flex;
}

.line {
    transition: all 1s;
}

#graph2.show {
    display: block;
}

svg {
    max-width: 100%;
    height: auto;
    background-color: aliceblue;
  }
</style>
