<script>
    import Banner from "$lib/Banner.svelte"; 
    import { onDestroy, onMount } from "svelte";

    const DUCK_IMAGES = {
        default: "/default.png",
        sleep: "/sleep.png",
        WHAT: "/WHAT.png",
        joy: "/joy.png",
        love: "/love.png",
        body: "/body.png"
    }

    let currentFaceSrc = DUCK_IMAGES.default;
    let duckMessage = "play with ducky!";
    let duckX = 0;
    let duckY = 0;

    let timeout;

    function updateFace(newSrc){
        currentFaceSrc = newSrc;
    }

    function updateMessage(newMessage){
        duckMessage = newMessage;
    }

    function duckClick(){
        clearTimeout(timeout);

        updateFace(DUCK_IMAGES.joy);
        updateMessage("ducky is happy!");

        timeout = setTimeout(() => {
            updateFace(DUCK_IMAGES.default);
            updateMessage("play with ducky!");
        }, 1800);
    }

    function goBack(){
        window.history.back();
    }

    onMount(() => {
        updateFace(DUCK_IMAGES.default);
        updateMessage("play with ducky!");
    });

    onDestroy(() => {
        clearTimeout(timeout);
    }


</script>

<Banner />

<main> 
    <h1> You found the secret page!</h1>
    <p>As your reward, you get to play with Ducky</p>
    <button on:click={() => window.history.back()}> Go back </button>
</main>

<style>
    main{
        text-align: center;
        padding: 2rem;
    }
</style>