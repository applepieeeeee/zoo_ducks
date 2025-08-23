<script>
    import {base} from "$app/paths";
    import Banner from "$lib/Banner.svelte";
    import { onDestroy, onMount } from "svelte";

    let images = [
        {src: `${base}/duck1.jpg`}, 
        {src: `${base}/duck2.jpg`},
        {src: `${base}/duck3.jpg`},
        {src: `${base}/duck4.jpg`},
        {src: `${base}/duck5.jpg`},
        {src: `${base}/duck6.jpg`},
        {src: `${base}/duck7.jpg`},
        {src: `${base}/duck8.jpg`},
        {src: `${base}/duck9.jpg`},
        {src: `${base}/duck10.jpg`},
        {src: `${base}/duck11.jpg`},
        {src: `${base}/duck12.jpg`}
    ]

    let quackSound;

    onMount(() =>{
        quackSound = new Audio(`${base}/quack.mp3`);
        document.addEventListener('click', playQuack);
    });

    onDestroy(() => {
        if (quackSound) document.removeEventListener('click', playQuack);
    });

    function playQuack() {
        if (quackSound){
            quackSound.currentTime = 0;
            quackSound.play();
        }
    }
</script>

<svelte:head>
    <title> Duck Images </title>
</svelte:head>

<Banner />

<main>
    <h1> Duck Image Gallery </h1>
    <div class = "gallery">
        {#each images as image}
            <div class = "image-container">
                <img src = {image.src} alt = {image.alt} />
            </div>
        {/each}
    </div>
    <div class = "back-button-container">
        <a href = "/">Go back to the main page </a>
    </div>
</main>

<style>
    :root{
        --bg-color: #ebe4dc;
        --card-bg: #fffefd;
        --text-color: #3f2406;
        --accent-color: #eec16f;
    }

    main{
        min-height: 100vh;
        background-color: var(--bg-color);
        padding: 2rem;
        text-align: center;
        font-family: 'Google Sans Code', monospace;
    }

    h1{
        color: var(--accent-color);
        font-size: 3rem;
        margin-bottom: 2rem;
    }

    .gallery{
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 1.6rem;
        max-width: 1000px;
        margin: 1rem auto;
    }

    .image-container{
        background-color: var(--card-bg);
        border-radius: 12px;
        padding: 1rem;
    }

    .image-container:hover{
        transform: scale(1.05);
        transform: translateY(-5px);
    }

    .image-container img{
        width: 100%;
        height: auto;
        border-radius: 8px;
    }

    .image-container p{
        margin-top: 1rem;
        color: var(--text-color);
        font-size: 1rem;
    }

    .back-button-container{
        margin-top: 3rem;
    }

    .back-button-container a{
        background-color: #68735b;
        color: white;
        text-decoration: none;
        padding: 12px 24px;
        border-radius: 10px;
    }

    .back-button-container a:hover{
        background-color: #4c5e40;
        transform: scale(1.04);
    }

</style>