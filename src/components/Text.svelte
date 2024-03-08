<script>
    import { fly, fade } from "svelte/transition";
    import { cubicInOut } from 'svelte/easing';

    export let index;

    const width = 500;
    const height = 500;
    
    let introIndex = 0;
    let isIntroVisible= false;
    let situationIndex1 = 1;
    let isSituation1Visible=false;
    let situationIndex2 = 2;
    let isSituation2Visible=false;
    let situationIndex3 = 3;
    let isSituation3Visible=false;
    let contIndex = 5;
    let isContVisible= false;

    $: if (index === introIndex) {
        isIntroVisible = true;
    } else {
        isIntroVisible = false;
    }

    $: if (index === situationIndex1) {
        isSituation1Visible = true;
    } else {
        isSituation1Visible = false;
    }

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

    $: if (index === contIndex) {
        isContVisible = true;
    } else {
        isContVisible = false;
    }
</script>

<div id='intro' class:show={isIntroVisible}>
    <svg width="800" height="400" viewBox="-1 0 {width+500} {height-300}" id='intro'>
        {#if isIntroVisible}
            <text 
                font-size="40px" 
                transform='translate(350, 100)'
                in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}}
                out:fly={{ delay: 250, duration: 750, x: -500, opacity: 0}}
            >
                Visualizing Rationalizibility:
            </text>

            <text 
                font-size="40px" 
                transform='translate(70, 200)'
                in:fly={{ delay: 250, duration: 750, x: 500, opacity: 0.0}}
                out:fly={{ delay: 250, duration: 750, x: 500, opacity: 0}}
            >
                Title w/ hook
            </text>
        {/if}
    </svg>
</div>

<div id='situation1' class:show={isSituation1Visible}>
    {#if isSituation1Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}}>Imagine that you are the owner of an ice cream shop and you find yourself in a competitive situation with a new ice cream shop that has opened just across the street.</p>
    {/if}
    {#if isSituation1Visible}
        <svg 
            class="icecream"
            id='player1'
            stroke="#000" stroke-opacity="0.2"
            xmlns="http://www.w3.org/2000/svg" data-name="Layer 2" viewBox="0 0 1271.22 949.58"
            in:fly={{ delay: 0, duration: 750, x: 500, opacity: 0}}
        >
            <defs><style>.cls-1{fill:#fbce58;}.cls-2{fill:#fff;}.cls-3{fill:#f06ca9;}.cls-4{fill:#9bd5c5;}</style></defs>
            <polygon class="cls-1" points="460.22 425.79 269.72 943.79 79.22 425.79 460.22 425.79"/>
            <path class="cls-2" d="M393.5,1273.79,200.13,748H586.87l-1,2.69ZM205.87,752,393.5,1262.21,581.13,752Z" transform="translate(-123.78 -324.21)"/>
            <rect class="cls-2" x="207.37" y="764.04" width="42.06" height="4" transform="translate(-627.3 120.47) rotate(-49.69)"/>
            <rect class="cls-2" x="219.43" y="801.02" width="147.14" height="4" transform="translate(-612.61 133.22) rotate(-46.11)"/>
            <rect class="cls-2" x="242.45" y="877.5" width="356.35" height="4" transform="translate(-631.25 256.81) rotate(-46.61)"/>
            <polygon class="cls-2" points="203.64 761.55 200.75 758.78 418.42 531.87 315.28 428.67 147.72 609.4 144.78 606.69 315.17 422.91 424.02 531.81 203.64 761.55"/>
            <rect class="cls-2" x="323.58" y="1085.07" width="189.42" height="4" transform="translate(-772.94 296.77) rotate(-45.37)"/>
            <rect class="cls-2" x="371.01" y="1207.06" width="60.48" height="4" transform="translate(-853.19 297.98) rotate(-44.21)"/>
            <rect class="cls-2" x="353.38" y="693.6" width="4" height="371.8" transform="translate(-646.96 197.59) rotate(-45.84)"/>
            <rect class="cls-2" x="544.46" y="741.23" width="4" height="61.28" transform="translate(-507.96 279.44) rotate(-44.46)"/>
            <rect class="cls-2" x="417.31" y="707.97" width="4" height="267.3" transform="translate(-604.67 245.79) rotate(-46.73)"/>
            <rect class="cls-2" x="353.71" y="829.61" width="4" height="296.33" transform="matrix(0.69, -0.72, 0.72, 0.69, -719.16, 231.66)"/>
            <rect class="cls-2" x="369.43" y="1006.82" width="4" height="175.24" transform="translate(-804.09 291.05) rotate(-46.76)"/>
            <rect class="cls-2" x="384.51" y="1182.9" width="4" height="56.51" transform="translate(-893.47 358.29) rotate(-47.73)"/>
            <path class="cls-3" d="M178,589S142.85,419.11,293.13,350.66c64.86-29.53,139.7-29.33,203.23,3C555.72,383.81,615.29,447,607,579c0,0,66,15.33,66,96.67s-110,116-170,78.66c0,0-28,65.34-114,65.34,0,0-65.33-.67-102.67-66.67,0,0-115.23,61.18-156.75-50.82-9.2-24.83-7.57-52.58,5.71-75.49C143.38,612.73,156.57,598.85,178,589Z" transform="translate(-123.78 -324.21)"/>
        </svg>
    {/if}
</div>
<!-- <div id='situation2' class:show={isSituation2Visible}>
        <svg 
            class="icecream"
            id='player1'
            stroke="#000" stroke-opacity="0.2"
            xmlns="http://www.w3.org/2000/svg" data-name="Layer 2" viewBox="0 0 1271.22 949.58"
        >
            <defs><style>.cls-1{fill:#fbce58;}.cls-2{fill:#fff;}.cls-3{fill:#f06ca9;}.cls-4{fill:#9bd5c5;}</style></defs>
            <polygon class="cls-1" points="460.22 425.79 269.72 943.79 79.22 425.79 460.22 425.79"/>
            <path class="cls-2" d="M393.5,1273.79,200.13,748H586.87l-1,2.69ZM205.87,752,393.5,1262.21,581.13,752Z" transform="translate(-123.78 -324.21)"/>
            <rect class="cls-2" x="207.37" y="764.04" width="42.06" height="4" transform="translate(-627.3 120.47) rotate(-49.69)"/>
            <rect class="cls-2" x="219.43" y="801.02" width="147.14" height="4" transform="translate(-612.61 133.22) rotate(-46.11)"/>
            <rect class="cls-2" x="242.45" y="877.5" width="356.35" height="4" transform="translate(-631.25 256.81) rotate(-46.61)"/>
            <polygon class="cls-2" points="203.64 761.55 200.75 758.78 418.42 531.87 315.28 428.67 147.72 609.4 144.78 606.69 315.17 422.91 424.02 531.81 203.64 761.55"/>
            <rect class="cls-2" x="323.58" y="1085.07" width="189.42" height="4" transform="translate(-772.94 296.77) rotate(-45.37)"/>
            <rect class="cls-2" x="371.01" y="1207.06" width="60.48" height="4" transform="translate(-853.19 297.98) rotate(-44.21)"/>
            <rect class="cls-2" x="353.38" y="693.6" width="4" height="371.8" transform="translate(-646.96 197.59) rotate(-45.84)"/>
            <rect class="cls-2" x="544.46" y="741.23" width="4" height="61.28" transform="translate(-507.96 279.44) rotate(-44.46)"/>
            <rect class="cls-2" x="417.31" y="707.97" width="4" height="267.3" transform="translate(-604.67 245.79) rotate(-46.73)"/>
            <rect class="cls-2" x="353.71" y="829.61" width="4" height="296.33" transform="matrix(0.69, -0.72, 0.72, 0.69, -719.16, 231.66)"/>
            <rect class="cls-2" x="369.43" y="1006.82" width="4" height="175.24" transform="translate(-804.09 291.05) rotate(-46.76)"/>
            <rect class="cls-2" x="384.51" y="1182.9" width="4" height="56.51" transform="translate(-893.47 358.29) rotate(-47.73)"/>
            <path class="cls-4" d="M178,589S142.85,419.11,293.13,350.66c64.86-29.53,139.7-29.33,203.23,3C555.72,383.81,615.29,447,607,579c0,0,66,15.33,66,96.67s-110,116-170,78.66c0,0-28,65.34-114,65.34,0,0-65.33-.67-102.67-66.67,0,0-115.23,61.18-156.75-50.82-9.2-24.83-7.57-52.58,5.71-75.49C143.38,612.73,156.57,598.85,178,589Z" transform="translate(-123.78 -324.21)"/>
        </svg>
</div> -->

<div id='situation2' class:show={isSituation2Visible}>
    {#if isSituation2Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}}>Every day before opening, both you and your rival must decide on the price at which you will sell your ice cream for the day. </p>
        <p in:fly={{ delay: 500, duration: 750, x: 500, opacity: 0}}>The choices are simple:</p>
        <li in:fly={{ delay: 1250, duration: 750, x: 500, opacity: 0}}>higher price, or</li> 
        <li in:fly={{ delay: 1900, duration: 750, x: 500, opacity: 0}}>a lower price.</li>
    {/if}
</div>

<div id='situation3' class:show={isSituation3Visible}>
    {#if isSituation3Visible}
        <p in:fly={{ delay: 0, duration: 750, x: -500, opacity: 0}} >As a rational and strategic individual, you understand that your decisions should be based on what you anticipate your rival might do, with the ultimate goal of maximizing your own profits by the end of the day.</p>
    {/if}
</div>

<div id='cont' class:show={isContVisible}>
    <p>
        Now imagine that rather than choosing only between a higher or lower price, you can choose exactly how much your ice cream costs.
    </p>
    <p>
        These days the industry standard price for a scoop of ice cream is somewhere between [0 and 6] dollars.
    </p>
</div>

<style>
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

    .icecream {
        scale: 0.5;
    }

</style>