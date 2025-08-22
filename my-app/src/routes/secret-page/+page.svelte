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
        justify-content: center;
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

    p{
        font-size: 1.25rem;
        color: #d7a349;
    }

    .ducky-hightlight{
        font-weight: 700;
        color: #e1c94f;
    }

    .duck-interaction-area{
        position: relative;
        width: 12rem;
        height: 12rem;

        margin-left: auto;
        margin-right: auto;
        margin-bottom: 2rem;

        transition: all 0.3s ease-in-out;
        transform: translate(var(--duck-x, 0px), var(--duck-y, 0px));
    }

</style>

<div class = "main-page-wrapper">
    <main>
        <h1> You found the secret page! </h1>
        <p> As your rewards, you get to play with <span class = "ducky-hightlight"> Ducky </span></p>
    </main>
</div>