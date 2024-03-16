<script>
    import { fly, fade } from "svelte/transition";
    import { cubicInOut } from 'svelte/easing';
    import { color } from "d3";

    export let index;

    const width = 500;
    const height = 500;

    let transitionIndex = 2;
    let transitionVisible = false;
    
    let situationIndex1 = 4;
    let isSituation1Visible=false;
    let situationIndex2 = 5;
    let isSituation2Visible=false;
    let situationIndex3 = 6;
    let isSituation3Visible=false;
    let situationIndex4 = 7;
    let isSituation4Visible= false;

    $: if (index === transitionIndex) {
        transitionVisible = true;
    }
    // } else {
    //     transitionVisible = false;
    // }

    $: if (index === situationIndex1) {
        isSituation1Visible = true;
    }
    // } else {
    //     isSituation1Visible = false;
    // }

    $: if (index === situationIndex2) {
        isSituation2Visible = true;
    } else {
        isSituation2Visible = false;
    }

    $: if (index === situationIndex3) {
        isSituation3Visible = true;
    } else {
        isSituation3Visible = false;
    }

    $: if (index === situationIndex4) {
        isSituation4Visible = true;
    } else {
        isSituation4Visible = false;
    }
</script>

<div id="transition" class:show={transitionVisible}>
    {#if index === transitionIndex}
        <p 
            in:fly={{delay: 200, duration: 750, x: -500, opacity: 0}}
            out:fly={{duration: 500, x: 700, opacity: 0}}
        >
            Consider the situation
        </p>
    {/if}
</div>

{#if index === situationIndex1}
    <div class="graph1_intro" class:show={isSituation1Visible}
        in:fly={{delay: 200, duration: 750, x: -500, opacity: 0}}
        out:fly={{duration: 500, x: -500, opacity: 0}}
    >
            <p>In this game of ice cream pricing, the concept of dominance plays a pivotal role in shaping your decisions and responses. A strategy is <b>dominant</b> if it outperforms all other available strategies, regardless of the choices made by your rival.</p>
            <p>We will focus on two strategies, setting a <mark class="player1">high price (H)</mark>, or setting a <mark class="player1">low price (L)</mark>.</p>
                <!-- <span class="dot"></span>
                <span class="dot"></span> -->
            <svg width=100% height=100px viewBox="0 0 {width} {height}" id="dots"> 
                <circle
                    fill=#f06ca9
                    cx=-300px
                    cy=300px
                    r="50"
                />
                <text 
                    fill=#f06ca9
                    font-size=150px
                    transform="translate(-200, 220)"
                >
                    H
                </text>

                <circle
                    fill=#f06ca9
                    cx=700px
                    cy=300px
                    r="50"
                />
                <text 
                    fill=#f06ca9
                    font-size=150px
                    transform="translate(770, 220)"
                >
                    L
                </text>
            </svg>
            <p>Scroll on to see the different types of dominance that can take place.</p>
    </div>
{/if}

{#if index === situationIndex2}
    <div class="graph1" class:show={isSituation2Visible}
        in:fly={{delay: 200, duration: 750, x: -500, opacity: 0}}
        out:fly={{duration: 500, x: -500, opacity: 0}}
    >
        <p>Consider the scenario where one pricing strategy consistently yields a higher profit than the other, irrespective of your rival's choice. In this case, that strategy is said to be <b>strictly dominant</b>. </p>
        <p>If setting a higher price consistently results in more profit, it is a dominant strategy for you. The dominance concept simplifies decision-making by allowing you to focus on the strategy that consistently performs better.</p>
    </div>
{/if}

{#if index === situationIndex3}
    <div class="graph1" class:show={isSituation3Visible}
        in:fly={{delay: 200, duration: 750, x: -500, opacity: 0}}
        out:fly={{duration: 500, x: -500, opacity: 0}}
    >
        <p>Now consider a scenario in which one pricing strategy yields a higher payoff than the other in some cases, and at least as high a payoff in all other cases, regardless of the choices made by your rival. In this case, the strategy is said to be <b>weakly dominant</b>.</p>
        <p>This strategy is preferred over other strategies in at least some situations but might not always be the best choice in all situations.</p>
    </div>
{/if}

{#if index === situationIndex4}
    <div class="graph1" class:show={isSituation4Visible}
        in:fly={{delay: 200, duration: 750, x: -500, opacity: 0}}
        out:fly={{duration: 500, x: -500, opacity: 0}}
    >
        <p><b>Visualizing dominance:</b></p>
        <p>Click on a dot and drag it to a new location on the grid.</p>
        <p>On the right, the payoff matrix for each potential outcome can be observed. <mark class="player1">Your choices</mark> are shown in each row of the matrix, and the <mark class="player2">your rival's choices</mark> are in each column, respectively. Each cell in the matrix represents the payoffs for each player, with <mark class="player1">your payoffs</mark> in the first position, and <mark class="player2">your rival's payoffs</mark> in the second position.</p>
    </div>
{/if}
<!-- <div id='situation1' class:show={isSituation1Visible}>
    {#if isSituation1Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}}>Imagine that you are the owner of an ice cream shop and you find yourself in a competitive situation with a new ice cream shop that has opened just across the street.</p>
        <p></p>
    {/if}
</div> -->

<!-- <div id='situation2' class:show={isSituation2Visible}>
    {#if isSituation2Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}}>Every day before opening, both you and your rival must decide on the price at which you will sell your ice cream for the day. </p>
        <p in:fly={{ delay: 500, duration: 750, x: 500, opacity: 0}}>The game is simple. Your choices are:</p>
        <li in:fly={{ delay: 1250, duration: 750, x: 500, opacity: 0}}>A higher price</li> 
        <p in:fly={{ delay: 1250, duration: 750, x: 500, opacity: 0}}>OR</p> 
        <li in:fly={{ delay: 1900, duration: 750, x: 500, opacity: 0}}>a lower price.</li>
    {/if}
</div>

<div id='situation3' class:show={isSituation3Visible}>
    {#if isSituation3Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}} >As a rational and strategic individual, you understand that your decisions should be based on what you anticipate your rival might do, with the ultimate goal of maximizing your own profits by the end of the day.</p>
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}} >Each player’s decision is unknown to the other until opening time. This is called a simultaneous game.</p>
    {/if}
</div> -->

<!-- <div id='cont' class:show={isContVisible}>
    <p>
        Now imagine that rather than choosing between discretely higher or lower price, you can choose exactly how much your ice cream costs.
    </p>
    <p>
        These days the industry standard price for a scoop of ice cream is somewhere between [1 and 4] dollars.
    </p>
</div> -->

<style>
    #transition p {
        /* width: 100%; */
        transform: translate(200px, 90px);
        text-align: center;
        color: #005792;
        font-size: 500%;
    }

    div {
        position: fixed;
        display: none;
        margin-top: 10vh;
    }

    p {
        width: 40vw;
        font-size: 1.5rem;
    }

    div.show {
        display: block;
    }

    div.graph1 {
        width: 20%;
        color:black;
        transform: translate(-150px, 100px);
        /* margin: 0 auto; */
        padding: 20px;
        border: 2px solid;
        border-color: #5585b5;
        border-radius: 10px;
        box-shadow: 20px -16px #5585b5;
        background-color: rgba(255, 255, 255);
        margin-bottom: 50px;
    }

    div.graph1_intro {
        width: 80%;
        color:black;

        transform: translate(-100px, 100px);
        /* margin: 0 auto; */
        padding: 20px;
        border: 2px solid;
        border-color: #5585b5;
        border-radius: 10px;
        box-shadow: 20px -16px #5585b5;
        background-color: rgba(255, 255, 255);
        margin-bottom: 50px;
    }

    div.graph1_intro p {
        width: 100%;
        font-size: larger;
        text-align: center;
    }

    div.graph1 p {
        width: 100%;
        font-size: 18px;
        text-align: center;
    }

    mark.player1 {
        background-color: transparent;
        color: #e35b99;
        font-weight: bold;
    }

    mark.player2 {
        background-color: transparent;
        color: #49c4a2;
        font-weight: bold;
    }

</style>