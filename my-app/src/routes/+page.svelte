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

    let fact_desc = $derived(facts[currentFact-1].description);
    let fact_title = $derived(facts[currentFact-1].title);

    onMount (function() {
        fetch(`${ base }/facts.json`)
        .then(response => response.json())
        .then(data => {
            facts.set(data);
            loaded = true;
            totalFacts = facts.length;
        });
    })


</script>

<svelte:head>

</svelte:head>

<!-- html -->
<main>

</main>


<!-- css -->
<style>

</style>