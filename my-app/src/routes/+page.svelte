<script>
    import { base } from "$app/paths";
    import { onMount } from "svelte";
    
    let loaded = $state(false); 

    let facts = $state([
        {
            "title": "Loading...",
            "description": "",
            "icon": "",
            "id": 0 
        }
    ]);
    
    let currentFact = $state(1);
    let totalFacts = $state(0);
    let viewedFacts = $state([]);
    let allViewed = $derived(viewedFacts.length === totalFacts && totalFacts > 0);

    let fact_desc = $derived(facts[currentFact - 1]?.description || "");
    let fact_title = $derived(facts[currentFact - 1]?.title || "");

    let fact_image = $derived(facts[currentFact - 1]?.image_url || "");

    onMount(async () => {
        const response = await fetch(`${base}/facts.json`);
        const data = await response.json();
        
        facts = data;
        loaded = true;
        totalFacts = facts.length;
        
    });

    function nextFact() {
        currentFact = currentFact % totalFacts === 0 ? 1 : currentFact + 1;
        const id = facts[currentFact - 1]?.id;
        if (id && !viewedFacts.includes(id)) {
            viewedFacts = [...viewedFacts, id];
        }
    }

    function prevFact() {
        currentFact = currentFact === 1 ? totalFacts : currentFact - 1;
        const id = facts[currentFact - 1]?.id;
        if (id && !viewedFacts.includes(id)) {
            viewedFacts = [...viewedFacts, id];
        }
    }
</script>

<svelte:head>
</svelte:head>

<main class="container">
    <div class="fact-card">
        <div class="fact-content">
            <div class="fact-icon">
                <img src="{base}/images/{fact_icon}" alt="{fact_title} image" />
            </div>
            <h2 class="fact-title">{fact_title}</h2>
            <p class="fact-description">{fact_desc}</p>
        </div>
        <div class="navigation-controls">
            <button on:click={prevFact} class="nav-button" aria-label="Previous fact">
                ◀
            </button>
            <span class="fact-counter">{currentFact} / {totalFacts}</span>
            <button on:click={nextFact} class="nav-button" aria-label="Next fact">
                ▶
            </button>
        </div>
    </div>
</main>

<style>
    :root {
        --bg-color: #f0f4f8;
        --card-bg: #fffefd;
        --text-color: #301b03;
        --accent-color: #f5c875;
    }

    main {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: var(--bg-color);
        font-family: Arial, sans-serif;
        padding: 1rem;
    }

    .fact-card {
        background-color: var(--card-bg);
        border-radius: 12px;
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

    .fact-icon img {
        width: 96px;
        height: 96px;
        margin-bottom: 1rem;
    }

    .fact-title {
        font-size: 1.8rem;
        font-weight: 700;
        margin-bottom: 0.5rem;
        color: var(--accent-color);
    }

    .fact-description {
        font-size: 1.1rem;
        line-height: 1.6;
        color: var(--text-color);
        margin-bottom: 2rem;
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
        border-radius: 50%;
        width: 44px;
        height: 44px;
        cursor: pointer;
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
        font-size: 1rem;
        font-weight: 600;
        color: #be6326;
    }
</style>