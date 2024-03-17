<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import { draw, fly, fade } from "svelte/transition";

    const width = 500;
    const height = 500;
    const marginTop = 0;
    const marginRight = 0;
    const marginBottom = 0;
    const marginLeft = 0;
    
    export let index;

    let isAxisVisible = false;

    let situationIndex1 = 4;
    let isSituation1Visible=false;
    let situationIndex2 = 5;
    let isSituation2Visible=false;
    let situationIndex3 = 6;
    let isSituation3Visible=false;

    $: if (index === situationIndex1) {
        isSituation1Visible = true; 
        isAxisVisible = false;
    }
    // } else {
    //     isSituation1Visible = false;
    // }

    $: if (index === situationIndex2) {
        isSituation2Visible = true;
        isAxisVisible = true;
    } else {
        isSituation2Visible = false;
    }

    $: if (index === situationIndex3) {
        isSituation3Visible = true;
        updateDots(2.5, 1, 2.5, 4)
    } else {
        isSituation3Visible = false;
    }

    let xScale = d3.scaleLinear([0, 5], [0, width]);
    const xAxis = [...Array(5).keys()];
    
    let yScale = d3.scaleLinear([0, 5], [0, height]);
    const yAxis = [...Array(5).keys()];

    let gx;
	let gy;

    $: x = d3.scaleLinear().domain([0, 5]).range([0, 500]);
    $: y = d3.scaleLinear().domain([0, 5]).range([500, 0]);
	$: d3.select(gy).call(d3.axisLeft(y));
	$: d3.select(gx).call(d3.axisBottom(x));

    // var gx = d3.scaleLinear()
    //     .domain([0, 5])
    //     .range([0, 500]);

    // var gy = d3.scaleLinear()
    //     .domain([0, 5])
    //     .range([500, 0]);

    let event1 = {'x':xScale(2.5), 'y':yScale(2.7), 'event': 'H'};
    let event2 = {'x':xScale(5), 'y':yScale(2.7), 'event': 'L'};
    let events = [event1, event2];
    let pmap = {'event1': event1, 'event2': event2}
    // let mounted = false;

    onMount(() => {
        // mounted = true;
        // d3.select("#example")
        //     .append("g")
        //     .attr("transform", "translate(0, 500)")
        //     .call(d3.axisBottom(gx));
        // d3.select("#example")
        //     .append("g")
        //     .attr("transform", "translate(0, 0)") 
        //     .call(d3.axisLeft(gy));
    });

    function updateDots(x1, y1, x2, y2) {
        event1['x'] = xScale(x1);
        event1['y'] = yScale(y1);
        event2['x'] = xScale(x2);
        event2['y'] = yScale(y2);

        d3.select("#event1")
            .transition()
            .attr("transform", "translate(" + (xScale(x1)) + "," + (yScale(y1)) + ")");
            

        d3.select("#event2")
            .transition()
            .attr("transform", "translate(" + (xScale(x2)) + "," + (yScale(y2)) + ")");
    }

</script>

{#if (index >= situationIndex2) && (index < 7)}
    <div class:show={isSituation1Visible} id="events" 
        in:fade={{delay: 200, duration: 200, opacity: 0}}
        out:fade={{duration: 200, opacity: 0}}
    >
        <svg {width} {height} viewBox="40 -40 {width-100} {height+100}" id="example">
            <g stroke="black" stroke-width="0.5" opacity=1>
                {#each xAxis as x}
                    {#if isAxisVisible}
                        <line x1="0" y1="0" x2="0" y2="{height}" transform='translate({xScale(x)} 0)' stroke='#d3d3d3' stroke-width='1' in:draw={{ duration: 1000 }}></line>
                    {/if}
                {/each}

                {#each yAxis as y}
                    {#if isAxisVisible}
                        <line x1="0" y1="{yScale(1)}" x2="{width}" y2="{yScale(1)}" transform='translate(0 {yScale(y)})' stroke='#d3d3d3' stroke-width='1' in:draw={{ duration: 1000 }}></line> 
                    {/if}
                {/each}
            </g>
            {#if isAxisVisible}
                <g color="black" bind:this={gx} transform="translate(0,500)" />
                <g color="black" bind:this={gy} transform="translate(0,0)" />
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
                    Payoff ($) if opposing player chooses a low price (L)
                </text>
            {/if}

            {#each events as d, i}
                <g class="points"
                    stroke="#ffe9e3" stroke-opacity="0"
                    key={i}
                    id='event{i+1}'
                    transform="translate({d.x}, {d.y})"
                    in:fly={{delay: 200, duration: 750, x: -500, opacity: 1}}
                    out:fly={{delay: 200, duration: 750, x: -500, opacity: 1}}         
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


            <!-- {#if index === situationIndex1}
                {updateDots(2.5, 2.7, 5, 2.7)}
            {/if} -->

            {#if index === situationIndex2}
                {updateDots(4, 2, 1, 4)}
            {/if}

            {#if index === situationIndex3}
                {updateDots(2.5, 1, 2.5, 4)}
            {/if}
        </svg>
        
        {#if index > situationIndex1}
        <div class="exampletable">
            <p class="p1_label">Player 2</p>
            <p class="p2_label">Player<br>1</p>
            <table border="1px">
                <tr>
                <td></td>
                <td><p class="p2">H</p></td>
                <td><p class="p2">L</p></td>
                </tr>
                <tr>
                <td><p class="p1">H</p></td>
                <td>
                    <p class="p1">${event1["x"] / 100}</p>
                    <p class="p2">${event1["x"] / 100}</p>
                </td>
                <td>
                    <p class="p1">${5 - (event1["y"] / 100)}</p>
                    <p class="p2">${event2["x"] / 100}</p>
                </td>
                </tr>
                <tr>
                <td><p class="p1">L</p></td>
                <td>   
                    <p class="p1">${pmap["event2"].x / 100}</p>
                    <p class="p2">${event2["x"] / 100}</p>
                </td>
                <td>
                    <p class="p1">${5 - (event2["y"] / 100)}</p>
                    <p class="p2">${5 - (event2["y"] / 100)}</p>
                </td>
                </tr>
            </table>
        </div>
        {/if}
    </div>
{/if}


<style>
    div {
        position: fixed;
        display: none;
        margin-top: 10vh;
    }

    div.show {
        display: block;
    }

    table {
        height: 100px;
        width: 250px;
        table-layout: fixed;
        border-collapse: collapse;
        border-color: #5585b5;
    }

    div.exampletable {
        display:inline-block;
    }

    #events {
        background-color: white;
        padding: 10px;
        border: 2px solid;
        border-color: #5585b5;
        border-radius: 10px;
        width: 870px;
        /* height: 600px; */
        transform: translate(220px, 30px);
        margin-left: 5vh;
    }

    table {
        height: 100px;
        width: 300px;
        border: 2px solid;
        border-color: #5585b5;
        table-layout: fixed;
        transform: translate(60px, -100px)
    }

    td {
        border: 2px solid;
        border-color: #5585b5;
        text-align: center;
        color: black
    }

    td p {
        display: inline-block;
        padding-left: 5px;
        padding-right: 5px;
    }


    .p1_label {
        color: #27c297;
        /* position: absolute;
        display:block;
        padding-right: 10%;*/
        transform: translate(60%, -40px);
    }
    .p2_label {
        color: #f06ca9;
        /* display: inline-block;
        padding-left: 10%; */
        text-align: center;
        transform: translate(-130px, 20px);
    }

    .p1 {
        color: #f06ca9;
        display: inline-block;
    }

    .p2 {
        color: #27c297;
    }


</style>