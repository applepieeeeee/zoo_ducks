<script>
    import Banner from "$lib/Banner.svelte";
    import { onDestroy, onMount } from "svelte";

    let images = [
        {src: `${base}/duck1.png`}
    ]

    let quackSound;

    onMount(() =>{
        quackSound = new Audio(`base/quack.mp3`);
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
        <a href = "/"></a> Go back to the main page </a>
    </div>
</main>

<style>
</style>