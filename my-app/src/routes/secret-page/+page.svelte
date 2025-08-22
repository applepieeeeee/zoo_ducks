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
    });

</script>

<Banner />

<main> 
    <h1> You found the secret page!</h1>
    <p>As your reward, you get to play with Ducky</p>
    <button on:click={() => window.history.back()}> Go back </button>
</main>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Ballet&display=swap');

    :root{
        --font1: 'Ballet', cursive;
    }

    :global(body){
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: 'Google Sans Code', monospace;
    }

    .main-page-wrapper {
        min-height: 100vh;
        background: #ebe6de;
        
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        padding: 2rem;
        position: relative;
        overflow: hidden;
    }

    main {
        font-family: 'Google Sans Code', monospace;
        text-align: center;
        padding: 2rem;

        box-sizing: border-box;
        background-color: white;

        border-radius: 3.12rem;
        max-width: 32rem;

        width: 100%;
        transition: transform 0.3s ease-in-out;
    }

    main:hover{
        transform: scale(1.05);
    }

    h1{
        font-size: 3rem;
        font-weight: 800;
        color: #b8813a;
        margin-bottom: 1rem;
        animation: fade-in 0.8s ease-out forwards;
    }

</style>