<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

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

    let player1 = {'x':xScale(2), 'y':yScale(2), 'event': 'A'};
    let player2 = {'x':xScale(3), 'y':yScale(3), 'event': 'B'};
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
            console.log("Alternative A strictly dominates B.");
            dominanceText = "Alternative A strictly dominates B.";
        } else if (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) {
            console.log("Alternative B strictly dominates A.");
            dominanceText = "Alternative B strictly dominates A.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y > pmap["player2"].y) || (pmap["player1"].x < pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative B weakly dominates A.");
            dominanceText = "Alternative B weakly dominates A.";
        } else if ((pmap["player1"].x == pmap["player2"].x && pmap["player1"].y < pmap["player2"].y) || (pmap["player1"].x > pmap["player2"].x && pmap["player1"].y == pmap["player2"].y)) {
            console.log("Alternative A weakly dominates B.");
            dominanceText = "Alternative A weakly dominates B.";
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

    $: if (index === 2) {
        isVisible = true;
    } else {
        isVisible = false;
    }

</script>
<div id='graph' class:show={isVisible}>
    <h3>Visualizing strict/weak dominance</h3>
    <p>Click on a dot and drag it to a new location on the grid. The visualization will dynamically update based on your movement.</p>

<svg {width} {height} viewBox="40 -40 {width-100} {height+100}" on:pointermove={recordMousePosition} id="chart1">
    <!-- axis -->
    <g stroke="#ffebeb" stroke-width="0.5" opacity=0>
        {#each xAxis as x}
            <line x="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(x)} 0)' stroke='black' stroke-width='1'></line>
        {/each}

            {#each yAxis as y}
                <line x="0" y1="{yScale(1)}" x2="{width}" y2="{yScale(1)}" transform='translate(0 {yScale(y)})' stroke='black' stroke-width='1'></line>
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
            Payoff if opposing player plays move A
        </text>

        <text
            class="ylabel"
            text-anchor="middle"
            transform="rotate(-90)"
            x={-250}
            y={-40}
        >
            Payoff if opposing player plays move B
        </text>
    </svg>
    

    <div class="tableChart1">
        <div class="p1-label"><p>Player 1</p></div>
        <div class = "tableP1">
            <div class="p2-label"><p>Player 2</p></div>
            <table border="1">
                <tr>
                <td></td>
                <td><div class="p2"><p>A</p></div></td>
                <td><div class="p2"><p>B</p></div></td>
                </tr>
                <tr>
                <td><div class="p1"><p>A</p></div></td>
                <td>
                    <div class="p1"><p>{pmap["player1"].x / 100}</p></div>
                    <div class="p2"><p>{pmap["player1"].x / 100}</p></div>
                </td>
                <td>
                    <div class="p1"><p>{height / 100 - pmap["player1"].y / 100}</p></div>
                    <div class="p2"><p>{pmap["player2"].x / 100}</p></div>
                </td>
                </tr>
                <tr>
                <td><div class="p1"><p>B</p></div></td>
                <td>   
                    <div class="p1"><p>{pmap["player2"].x / 100}</p></div>
                    <div class="p2"><p>{height / 100 - pmap["player1"].y / 100}</p></div>
                </td>
                <td>
                    <div class="p1"><p>{height / 100 - pmap["player2"].y / 100}</p></div>
                    <div class="p2"><p>{height / 100 - pmap["player2"].y / 100}</p></div>
                </td>
                </tr>
            </table>
        </div>
    </div>

    <div>
        <p>{dominanceText}</p>
    </div>  

</div>

<style>
    #graph {
        position:fixed;
        display: none;
    }

    #graph.show {
        display: block;
    }

    #graphTable {
        display: flex;
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

    /* .cls-1, .cls-2, .cls-3, .cls-4 {
        scale: 0.3;
    } */
    

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

</style>
