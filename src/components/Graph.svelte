<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    // import { circIn } from 'svelte/easing';

    const width = 500;
    const height = 500;
    const marginTop = 50;
    const marginRight = 50;
    const marginBottom = -50;
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

        d3.select("#chart1")
            .append("g")
            .attr("transform", "translate(0, 500)")
            .call(d3.axisBottom(gx));

        d3.select("#chart1")
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

    $: if (index === 1) {
        isVisible = true;
    } else {
        isVisible = false;
    }

</script>
<div id='graph' class:show={isVisible}>

    <h1>Rationalizibility</h1>
    <p>Visualizing strict/weak dominance</p>
    <p>Click on a dot and drag it to a new location on the grid. The visualization will dynamically update based on your movement.</p>

<svg {width} {height} viewBox="0 0 {width-65} {height+30}" on:pointermove={recordMousePosition} id="chart1">
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

svg {
    max-width: 100%;
    height: auto;
  }
</style>
