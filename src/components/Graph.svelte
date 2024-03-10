<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import { draw } from "svelte/transition";

    const width = 500;
    const height = 500;
    const marginTop = 0;
    const marginRight = 0;
    const marginBottom = 0;
    const marginLeft = 0;

    let dominanceText = "No dominance relationship."

    let xScale = d3.scaleLinear([0, 5], [0, width]);
    const xAxis = [...Array(5).keys()];
    
    let yScale = d3.scaleLinear([0, 5], [0, height]);
    const yAxis = [...Array(5).keys()];

    var gx = d3.scaleLinear()
        .domain([0, 5])
        .range([0, 500]);

    var gy = d3.scaleLinear()
        .domain([0, 5])
        .range([500, 0]);

    let player1 = {'x':xScale(2), 'y':yScale(2), 'event': 'H'};
    let player2 = {'x':xScale(3), 'y':yScale(3), 'event': 'L'};
    let players = [player1, player2];
    let pmap = {'player1': player1, 'player2': player2}

    let mousePosition = [0, 0];
    function recordMousePosition(event) {
        mousePosition = d3.pointer(event);
    }

    onMount(() => {
        d3.select("#chart1")
            .append("g")
            .attr("transform", "translate(0, 500)")
            .call(d3.axisBottom(gx));
        d3.select("#chart1")
            .append("g")
            .attr("transform", "translate(0, 0)") 
            .call(d3.axisLeft(gy));
        d3.selectAll('.points').call(d3.drag()
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

        const [snapX, snapY] = snapToGrid([d.x, d.y]);

        const maxX = width - marginLeft - marginRight;
        const maxY = height - marginTop - marginBottom;

        const clampedX = Math.max(Math.min(snapX, maxX), marginLeft);
        const clampedY = Math.max(Math.min(snapY, maxY), marginTop);


        d3.select(this)
            .attr("transform", "translate(" + (clampedX) + "," + (clampedY) + ")");

            // .attr("x", clampedX)
            // .attr("y", clampedY);
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
            console.log("Alternative H strictly dominates L.");
            dominanceText = "Alternative H strictly dominates L.";
        } else if (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) {
            console.log("Alternative L strictly dominates H.");
            dominanceText = "Alternative L strictly dominates H.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) || (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative L weakly dominates H.");
            dominanceText = "Alternative L weakly dominates H.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y < pmap["player2"].y) || (pmap["player1"].x > pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative H weakly dominates L.");
            dominanceText = "Alternative H weakly dominates L.";
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

    $: if (index === 4) {
        isVisible = true;
    } else {
        isVisible = false;
    }

</script>
<div id='graph1' class:show={isVisible}>
    <h3>Visualizing strict/weak dominance<br></h3>
    <p class="text">In the intricate game of ice cream pricing, the concept of <em>dominance</em> plays a pivotal role<br>in shaping your decisions and responses. <em>Dominance</em> in game theory refers to a strategy<br>that outperforms all other available strategies, regardless of the choices made by your rival.</p>
    <p class='text'>Click on a dot and drag it to a new location on the grid. <br/> The visualization will dynamically update based on your movement.</p>

    <div id="graphContainer">
        <svg {width} {height} viewBox="40 -40 {width-100} {height+100}" on:pointermove={recordMousePosition} id="chart1">
            <!-- axis -->
            <g stroke="#ffebeb" stroke-width="0.5" opacity=1>
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

            <!-- points -->
            {#each players as d, i}
                <g class="points"
                    stroke="#ffe9e3" stroke-opacity="0"
                    key={i}
                    id='player{i+1}'
                    transform="translate({d.x}, {d.y})"
                >
                    <circle
                        fill=#f06ca9
                        r="12"
                    />
                    <text 
                        fill=#f06ca9
                        font-size=30
                        transform="translate(15, -15)"
                    >
                        {d.event}
                    </text>
                </g>
            {/each}

            <text
                class="xlabel"
                text-anchor="middle"
                x={width-250}
                y={height+40}
            >
                Payoff ($) if opposing player chooses a high price (H)
            </text>

            <text
                class="ylabel"
                text-anchor="middle"
                transform="rotate(-90)"
                x={-250}
                y={-40}
            >
                Payoff ($) if opposing player chooses a low pirce (L)
            </text>
        </svg>
        

        <div style="display:flex; flex-direction:column; align-items:center">
            <div class="tableChart1">
                <div class="p1-label"><p>Player 1</p></div>
                <div class = "tableP1">
                    <div class="p2-label"><p>Player 2</p></div>
                    <table border="1">
                        <tr>
                        <td></td>
                        <td><div class="p2"><p>H</p></div></td>
                        <td><div class="p2"><p>L</p></div></td>
                        </tr>
                        <tr>
                        <td><div class="p1"><p>H</p></div></td>
                        <td>
                            <div class="p1"><p>${pmap["player1"].x / 100}</p></div>
                            <div class="p2"><p>${pmap["player1"].x / 100}</p></div>
                        </td>
                        <td>
                            <div class="p1"><p>${pmap["player1"].y / 100}</p></div>
                            <div class="p2"><p>${pmap["player2"].x / 100}</p></div>
                        </td>
                        </tr>
                        <tr>
                        <td><div class="p1"><p>L</p></div></td>
                        <td>   
                            <div class="p1"><p>${pmap["player2"].x / 100}</p></div>
                            <div class="p2"><p>${pmap["player1"].y / 100}</p></div>
                        </td>
                        <td>
                            <div class="p1"><p>${pmap["player2"].y / 100}</p></div>
                            <div class="p2"><p>${pmap["player2"].y / 100}</p></div>
                        </td>
                        </tr>
                    </table>
                </div>
            </div>

            <div>
                <p class="text" style="margin-left: 90px;">{dominanceText}</p>
                {#if dominanceText === "Alternative H strictly dominates L."}
                <p class="text" style="margin-left: 90px;">Player 1's strategy H yields a higher payoff<br>than any strategy of Player 2, regardless of<br>Player 2's choice.</p>
                {:else if dominanceText === "Alternative L strictly dominates H."}
                <p class="text" style="margin-left: 90px;">Player 2's strategy B yields a higher payoff<br>than any strategy of Player 1, regardless of<br>Player 1's choice.</p>
                {:else if dominanceText === "Alternative L weakly dominates H."}
                <p class="text" style="margin-left: 90px;">Player 2's strategy B yields a higher<br>payoff than any strategy of Player 1 when Player 1 plays L.</p>
                {:else if dominanceText === "Alternative H weakly dominates L."}
                <p class="text" style="margin-left: 90px;">Player 1's strategy A yields a higher<br>payoff than any strategy of Player 2 when Player 2 plays H.</p>
                {/if}
            </div>
        </div>

    </div>
</div>

<style>
    #graph1 {
        position:fixed;
        display: none;
        margin-top: 5vh;
        color: black;
    }

    #graph1.show {
        display: block;
    }

    #graphTable {
        display: flex;
    }

    h3, .text {
        color: white;
    }

    table {
        background-color: aliceblue;
        table-layout: fixed;
        border-collapse: collapse;
        height: 100px;
        width: 300px;
    }

    svg {
        background-color: aliceblue;
        position: relative;
        max-width: 100%;
        /* height: auto; */
    }

    circle:hover {
       filter: brightness(0.8);
       cursor: grab;
    }

    circle:active {
       cursor: grabbing;
    }

    

    td {
        text-align: center;
        padding:1%;
    }

    .p1 {
        color: #f06ca9;
        display: inline-block;
        padding-right: 10%;   
    }
    .p2 {
        color: #27c297;
        display: inline-block;
        padding-left: 10%;
    }

    .p2-label {
        margin-left: 45%;
        color: #27c297;
    }

    .p1-label {
        color: #f06ca9;
        margin-top: 35%;
        padding-right: 5%;
        text-align: center;
    }

    .tableChart1 {
        display:flex;
    }
    
    #graphContainer {
        display: flex;
    }

</style>
