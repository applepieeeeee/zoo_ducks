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

    let sleepTimer;
    let whatInterval;


    let petCount = 0;
    let lastInteractionTime = Date.now();

    function updateFace(newSrc){
        currentFaceSrc = newSrc;
    }

    function updateMessage(newMessage){
        duckMessage = newMessage;
    }

    function resetSleepTimer(){
        clearTimeout(sleepTimer);
        sleepTimer = setTimeout(() =>{
            if (Date.now() - lastInteractionTime >= 5000){
                updateFace(DUCK_IMAGES.sleep);
                updateMessage("zzzz ducky is sleeping....");
                petCount = 0;
            }
        }, 5000);
    }

    function startWhatTimer(){
        clearInterval(whatInterval);
        whatInterval = setInterval(() => {
            const random = Math.random();
            if (random < (1/60)){
                updateFace(DUCK_IMAGES.WHAT);
                updateMessage("WHAT??? HELLO?? oh it's you");
                petCount = 0;

                clearTimeout(timeout);
                timeout = setTimeout(() => {
                    updateFace(DUCK_IMAGES.default);
                    updateMessage("play with ducky!");
                }, 3000);
            }
        }, 1000);
    }

    function duckClick(){
        lastInteractionTime = Date.now();
        resetSleepTimer();
        clearTimeout(timeout);

        petCount++;

        if (petCount >= 10){
            updateFace(DUCK_IMAGES.love);
            updateMessage("ducky loves u sm");
            petCount = 0;
        } else {
            updateFace(DUCK_IMAGES.joy);
            updateMessage("ducky is happy!");
            timeout = setTimeout(() => {
                updateFace(DUCK_IMAGES.default);
                updateMessage("play with ducky!");
            }, 1800);
        }
    }

    function goBack(){
        window.history.back();
    }

    onMount(() => {
        updateFace(DUCK_IMAGES.default);
        updateMessage("play with ducky!");

        resetSleepTimer();
        startWhatTimer();
    });

    onDestroy(() => {
        clearTimeout(timeout);
        clearTimeout(sleepTimer);
        clearInterval(whatInterval);
    });

</script>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Ballet&display=swap');

    :root{
        --font1: 'Ballet', cursive;
    }

    :global(body){
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-family: var(--font1);
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
        font-weight: 900;
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

        background: none;
        border: none;
        padding: 0;
        cursor: pointer;
        outline: none;
    }

    .duck-image{
        position: absolute;

        top:0;
        left:0;
        width:100%;
        height:100%;

        object-fit:contain;
        border-radius: 8000px;
        transition: transform 0.5s;
    }

    .duck-face{
        top:0%;
        left: 0%;
        width: 100%;
        height: 100%;
        transition: filter 0.5s ease-in-out, transform 0.5s ease-in-out;
    }

    .duck-message-bubble{
        position: absolute;
        top: -2.5rem;
        background-color: #8ea8af;

        color: #fff;  
        font-size: 0.9rem;
        padding: 0.25rem 0.75rem;

        border-radius: 8000px;
        white-space: nowrap;
        transition: all 0.3s ease-in-out;
        opacity: 0.95;
        
        transform: translateX(-50%);
        left: 50%;

    }

    .go-back-button {
        background-color: #638485;
        color: #fff;

        font-weight: 700;
        padding: 0.8rem 1.5rem;
        
        border-radius: 1000px;
        box-shadow: 0 10px 15px -3px rgba(0,0,0,0.1), 0 4px 6px -2px rgba(0,0,0,0.05);

        transition: all 0.3s ease-in-out;
        outline: none;
        border: none;

        font-family: 'Google Sans Code', monospace;
    }
    
    .go-back-button:hover{
        background-color: #475c65;
        transform: scale(1.05);
    }

    .go-back-button:active{
        transform: scale(0.95);
    }

    @keyframes fade-in{
        from{ opacity: 0; transform: translateY(20px); }
        to{ opacity: 1; transform: translateY(0); }
    }

    .animate-fade-in{
        animation: fade-in 0.8s ease-out-forwards;
    }

</style>

<Banner />

<div class = "main-page-wrapper">
    <main>
        <h1> You found the secret page! </h1>
        <p> As your reward, you get to play with <span class = "ducky-hightlight"> Ducky </span></p>

        <button
            id = "duck-interaction-area"
            class = "duck-interaction-area"
            on:click = {duckClick}
            type = "button"
            aria-label = "Play with Ducky"
            style= "--duck-x: {duckX}; --duck-y: {duckY};"
        >

        <img
            src = {DUCK_IMAGES.body}
            alt = "ducky body"
            class = "duck-image"
            />

            <img
                src = {currentFaceSrc}
                alt = "ducky face :D"
                class = "duck-image duck-face"
            />

            <div class = "duck-message-bubble">
                {duckMessage}
            </div>
        </button>

        <p class = "instructions-text">
            Click Ducky to pet him!
        </p>

        <button on:click={goBack} class = "go-back-button"> Go back</button>
    </main>
</div>