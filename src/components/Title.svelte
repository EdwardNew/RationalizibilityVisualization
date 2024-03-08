<script>
    import { fly, fade } from "svelte/transition";
    import { cubicInOut } from 'svelte/easing';

    export let index;

    const width = 500;
    const height = 500;
    
    let introIndex = 0;
    let isIntroVisible= false;

    let situationIndex = 1;
    let isSituationVisible=false;

    let contIndex = 3;
    let isContVisible= false;

    $: if (index === introIndex) {
        isIntroVisible = true;
    } else {
        isIntroVisible = false;
    }

    $: if (index === situationIndex) {
        isSituationVisible = true;
    } else {
        isSituationVisible = false;
    }

    $: if (index === contIndex) {
        isContVisible = true;
    } else {
        isContVisible = false;
    }

    let iteration = 0;
    let range = [0,4];

    function iterate() {
        if (iteration < 3) {
            iteration++;
        }
        console.log('interate', iteration);
    }

    function back() {
        if (iteration > 0) {
            iteration--;
        }
        console.log('back', iteration)
    }
</script>

<div id='intro' class:show={isIntroVisible}>
    <svg width="1000" height="500" viewBox=" -300 200 {width+3000} {height+100}" id='intro'>
        {#if isIntroVisible}
            <text 
                class="title"
                font-size="300px" 
                transform='translate(540, 500)'
                in:fly={{ delay: 0, duration: 700, x: -300, opacity: 0}}
                out:fly={{ delay: 250, duration: 500, x: -500, opacity: 0}}
            >
                Visualizing
            </text>
            <text 
                class="title"
                font-size="300px" 
                transform='translate(-100, 750)'
                in:fly={{ delay: 0, duration: 700, x: -300, opacity: 0}}
                out:fly={{ delay: 250, duration: 500, x: -500, opacity: 0}}
            >
                Rationalizibility:
            </text>

            <text 
                class="title"
                font-size="200px" 
                transform='translate(-300, 1000)'
                in:fly={{ delay: 250, duration: 500, x: 500, opacity: 0.0}}
                out:fly={{ delay: 250, duration: 500, x: 500, opacity: 0}}
            >
                The Art of Decision-Making
            </text>
        {/if}
    </svg>
</div>


<style>
    #intro {
        left: 350px;
        bottom: 70px;
        overflow: hidden;
    }

    .title {
        fill: #f8fcfb;
    }
    div {
        position: fixed;
        display: none;
    }

    p {
        width: 50vw;
    }

    div.show {
        display: block;
    }


</style>