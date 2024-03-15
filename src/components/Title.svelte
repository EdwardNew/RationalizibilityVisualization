<script>
    import { fly, fade } from "svelte/transition";
    import { cubicInOut } from 'svelte/easing';

    export let index;

    const width = 500;
    const height = 500;
    
    let introIndex = 0;
    let isIntroVisible= false;

    $: if (index === introIndex) {
        isIntroVisible = true; }
    // } else {
    //     isIntroVisible = false;
    // }

</script>

<div id='intro' class:show={isIntroVisible}>
    <svg width="1000" height="500" viewBox=" -500 500 {width+2500} {height+100}" id='intro'>
        {#if index == 0}
            <rect 
                width=4500px 
                height=3000px 
                transform='translate(-2000, 100)'
                fill=#5585b5
                in:fade={{duration: 200, opacity: 0}}
                out:fade={{delay: 200, duration: 200, opacity: 0}}
            />
            <text 
                class="title"
                font-size="300px" 
                transform='translate(850, 600)'
                in:fly={{ duration: 700, x: -300, opacity: 0}}
                out:fly={{duration: 500, x: -500, opacity: 0}}
            >
                Visualizing
            </text>
            <text 
                class="title"
                font-size="300px" 
                transform='translate(150, 900)'
                in:fly={{ duration: 700, x: -300, opacity: 0}}
                out:fly={{ duration: 500, x: -500, opacity: 0}}
            >
                Rationalizibility:
            </text>

            <text 
                class="title"
                font-size="200px" 
                transform='translate(-350, 1250)'
                in:fly={{ delay: 150, duration: 500, x: 500, opacity: 0.0}}
                out:fly={{ delay: 150, duration: 500, x: 500, opacity: 0}}
            >
                The Art of Decision-Making
            </text>
        {/if}
    </svg>
</div>


<style>
    #intro {
        left: 210px;
        top: 110px;
        overflow: hidden;
        /* background-color: #c06c84;
        border-radius: 50px; */
    }

    .title {
        fill: #f8fcfb;
    }
    div {
        position: absolute;
        display: none;
    }

    div.show {
        display: block;
    }


</style>