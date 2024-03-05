<script>
    export let index;
    let introIndex = 0;
    let isIntroVisible= false;

    let contIndex = 2;
    let isContVisible= false;

    $: if (index === introIndex) {
        isIntroVisible = true;
    } else {
        isIntroVisible = false;
    }

    $: if (index === contIndex) {
        isContVisible = true;
    } else {
        isContVisible = false;
    }
</script>

<div id='intro' class:show={isIntroVisible}>
    <p>
        Imagine that you are the owner of an ice cream shop and you find yourself in a competitive situation with a new ice cream shop that has opened just across the street. Every day before opening, both you and your rival must decide on the price at which you will sell your ice cream for the day. The choices are simple: a higher price or a lower price. Being rational and strategic individuals, you understand that your decisions should be based on what you anticipate your rival might do, with the ultimate goal of maximizing your own profits by the end of the day.
    </p>
</div>

<div id='cont' class:show={isContVisible}>
    <p>
        Now imagine that rather than choosing only between a higher or lower price, you can choose exactly how much your ice cream costs.<br/>
        These days the industry standard price for a scoop of ice cream is somewhere between [2 and 8] dollars.
    </p>
</div>

<style>

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