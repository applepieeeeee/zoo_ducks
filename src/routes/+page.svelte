
<script>
    import { base } from "$app/paths";
    import { onDestroy, onMount } from "svelte";
    import Banner from "$lib/Banner.svelte"; 
    
    let loaded = $state(false); 

    let facts = $state([
        {
            "description": "",
            "image_url": "",
            "id": 0 
        }
    ]);

    let currentFact = $state(1);
    let totalFacts = $state(0);
    let viewedFacts = $state([]);

    
    let shareMsg = $state('');

    let allViewed = $derived(viewedFacts.length === totalFacts && totalFacts > 0);

    let fact_desc = $derived(facts[currentFact - 1]?.description || "");
    let fact_title = $derived(facts[currentFact - 1]?.title || "");
    let fact_image = $derived(facts[currentFact - 1]?.image_url || "");

    let quackSound;
    
    onMount(async () => {
        const response = await fetch(`${base}/facts.json`);
        const data = await response.json();
        
        facts = data;
        loaded = true;
        totalFacts = facts.length;

        quackSound = new Audio(`${base}/quack.mp3`);
        document.addEventListener('click', playQuack);
    });

    onDestroy(() => {
        if (quackSound){
            document.removeEventListener('click', playQuack);
        }
    });

    function playQuack(){
        if (quackSound){
            quackSound.currentTime = 0;
            quackSound.play();
        }
    }

    function nextFact() {
        currentFact = currentFact % totalFacts === 0 ? 1 : currentFact + 1;
        const id = facts[currentFact - 1]?.id;
        if (id && !viewedFacts.includes(id)) {
            viewedFacts = [...viewedFacts, id];
        }
        shareMsg = '';
    }

    function prevFact() {
        currentFact = currentFact === 1 ? totalFacts : currentFact - 1;
        const id = facts[currentFact - 1]?.id;
        if (id && !viewedFacts.includes(id)) {
            viewedFacts = [...viewedFacts, id];
        }
        shareMsg = '';
    }

    async function shareFact(){
        const textToCopy = `🦆 Duck Fact: ${fact_title}\n${fact_desc} 🦆`;
        try{
            const textarea = document.createElement('textarea');
            textarea.value = textToCopy;
            textarea.style.position = 'fixed';
            textarea

            document.body.appendChild(textarea);

            textarea.focus();
            textarea.select();

            document.execCommand('copy');
            document.body.removeChild(textarea);
            shareMsg = 'Copied to clipboard!';
        } catch (err){
            console.error('Failed to copy: ', err);
            shareMsg = 'Failed to copy :(';
        }
        setTimeout(() => { shareMsg = ''; }, 4000);
    }

</script>

<Banner />

<svelte:head>
</svelte:head>

<main class="container">
     <h1 class = "title">
        <span class = "title-letter">D</span>
        <span class = "title-letter">u</span>
        <span class = "title-letter">c</span>
        <span class = "title-letter">k</span>
        <span class = "title-letter">s</span>
        <span class = "title-letter">!</span>
    </h1>

    <h2 class = "subtitle"> Explore this page to learn more about ducks! </h2>

    <div class="fact-card">

        <div class="fact-content">
            <div class="fact-image">
                <img src={base + fact_image} alt="Image related to {fact_title}" />
            </div>

            <h2 class="fact-title">{fact_title}</h2>
            <p class="fact-description">{fact_desc}</p>
        </div>
    
        <div class="navigation-controls">
            <button onclick={prevFact} class="nav-button" aria-label="Previous fact">
                ◀
            </button>

            <span class="fact-counter">{currentFact} / {totalFacts}</span>

            <button onclick={nextFact} class="nav-button" aria-label="Next fact">
                ▶
            </button>
            
        </div>

        {#if shareMsg}
            <div class="share-message">{shareMsg}</div>
        {/if}

        <div class = "share-section">
            <button onclick = {shareFact} class = "share-button"> Share Fact </button>
        </div>

        {#if allViewed}
            <div class="secret-page-unlock">
                <p>You've viewed all the facts!</p>
                <button onclick={() => window.location.href = '/secret-page'}>Open secret page</button>
            </div>
        {/if}

    </div>

    <div class = "link-section">
        <a href = "/duck-images" class = "link-button"> View Duck Images </a>
    </div>
</main>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Ballet&display=swap');

    :root {
        --bg-color: #ecf4f7;
        --card-bg: #fffefd;
        --text-color: #301b03;
        --accent-color: #f5c875;
    }

    main {
        min-height: 100vh;

        display: flex;
        flex-direction: column;

        justify-content: center;
        align-items: center;
        text-align: center;
        
        background-color: var(--bg-color);

        font-family: 'Google Sans Code', monospace;
        padding: 1rem;

        margin-bottom: -10px;
        margin-top: -22px;
        margin-left: -20px;
    }

    .fact-card {
        background-color: var(--card-bg);

        border-radius: 20px;
        padding: 3rem;

        max-width: 500px;
        width: 100%;

        text-align: center;
        transition: transform 0.3s ease;
    }

    .fact-card:hover {
        transform: translateY(-5px);
    }

    .fact-content {
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .fact-image img {
        width: 100%;
        max-width: 300px;
        height: auto;

        border-radius: 20px;
        margin-bottom: 1rem;
    }

    .fact-title {
        font-family: 'Google Sans Code', monospace;

        font-size: 3rem;
        font-weight: 700;
        letter-spacing: 0.06em;

        margin-bottom: -1rem;
        margin-top: 0rem;
        color: var(--accent-color);
    }

    .fact-description {
        font-size: 1.1rem;
        line-height: 1.6;
        color: var(--text-color);
        margin-bottom: 2rem;

        font-family: 'Google Sans Code', monospace;
    }

    .navigation-controls {
        display: flex;
        justify-content: space-between;
        align-items: center;

        width: 100%;
        margin-top: 1rem;
    }

    .nav-button {
        background-color: var(--accent-color);
        color: white;

        border: none;
        border-radius: 45%;

        width: 44px;
        height: 44px;

        display: flex;
        justify-content: center;
        align-items: center;

        transition: background-color 0.3s ease, transform 0.2s ease;
    }

    .nav-button:hover {
        background-color: #f0b549;
        transform: scale(1.1);
    }

    .fact-counter {
        font-size: 1.6rem;
        font-weight: 600;
        color: #be6326;
    }

    .secret-page-unlock {
        margin-top: 0rem;
        margin-bottom: -1rem;
        text-align: center;
        padding: 2rem;
        border-radius: 20px;
    }
    
    .secret-page-unlock button {
        background-color: #eeb199;
        color: white;

        border: 1px solid #e6a991; 
        padding: 12px 24px; 
        border-radius: 10px; 

        font-size: 1.1rem;
        font-family: 'Google Sans Code', monospace;

        transition: background-color 0.3s ease, transform 0.3s ease;
    }

    .secret-page-unlock button:hover{
        background-color: #b84915;
    }
    
    .title {
        font-family: 'Google Sans Code', monospace;
        font-size: 6rem;
        font-weight: 800;

        color: var(--accent-color);

        margin-top: 0rem;
        margin-bottom: 1rem;
        display: flex;
        justify-content: center;
    }

    .title-letter{
        transition: color 0.3s ease;
        display: inline-block;
        cursor: pointer;
        padding: 0 0.5rem;
    }

    .title-letter:hover{
        color: #be6326;
        transform: translateY(-3px);
    }

    .subtitle{
        font-family: 'Google Sans Code', monospace;
        font-size: 1.8rem;
        font-weight: 500;
        color: #cd8834;
        
        margin-top: 0;
        margin-bottom: 2rem;

        opacity: 0;
        animation: fadeIn 2s ease-in-out forwards;
    }

    @keyframes fadeIn{
        from{
            opacity: 0;
            transform: translateY(20px);
        } 
        to{
            opacity: 1;
            transform: translateY(0);
        }
    }

    .share-section{
        margin-top: 2rem;
        margin-bottom: 2rem;
    }

    .share-button{
        background-color: #687e5b;
        color: white;

        padding: 12px 24px;
        border-radius: 10px;
        border: 1px solid #687e5b;

        font-size: 1.1rem;

        font-family: 'Google Sans Code', monospace;
        transition: background-color 0.2s ease, transform 0.2s ease;
    }

    .share-button:hover{
        background-color: #4c5e40;
        transform: scale(1.04);
        border: 1px solid #687e5b;
    }

    .share-message{
        margin-top: 1rem;
        padding: 0.5rem 1rem;
        background-color: #b0caa1;
        color: white;

        border-radius: 5px;
        font-size: 0.92rem;
        text-align: center;

        animation: fadeOut 3s forwards;
    }

    @keyframes fadeOut{
        0%{
            opacity: 1;
        }
        60%{
            opacity: 1;
        }
        100%{
            opacity: 0;
            display: none;
        }
    }

    .link-section{
        margin-top: 1rem;
        margin-bottom: 2rem;
    }

    .link-button{
        background-color: #9c7362;
        color: white;
        text-decoration: none;
        padding: 12px 24px;
        
        border-radius: 10px;
        border: 1px solid #9c7362;

        font-size: 1.1rem;
        font-family: 'Google Sans Code', monospace;
        transition: background-color 0.3s ease, transform 0.2s ease;
        display: inline-block;

        margin-top: 1rem;
    }

    .link-button:hover{
        background-color: #815948;
        transform: scale(1.04);
    }
</style>