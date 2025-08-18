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

    let fact_desc = $derived(facts[currentFact-1].description);
    let fact_title = $derived(facts[currentFact-1].title);

    onMount (function() {
        fetch(`${ base }/facts.json`)
        .then(response => response.json())
        .then(data => {
            facts = data;
            loaded = true;
            totalFacts = facts.length;
        });
    })

    function nextFact() {
        if (currentFact < totalFacts){
            currentFact += 1;
        } else {
            currentFact = 1;
        }
            const id = facts[currentFact-1]?.id;
            if (id && !viewedFacts.includes(id)) {
                viewedFacts = [...viewedFacts, id];
            }
    }

    function prevFact(){
        if (currentFact > 1) {
            currentFact -= 1;
        } else {
            currentFact = totalFacts;
        }
            const id = facts[currentFact-1]?.id;
            if (id && !viewedFacts.includes(id)) {
                viewedFacts = [...viewedFacts, id];
            }
    }


</script>

<svelte:head>

</svelte:head>

<!-- html -->
<main>
    <h1> Ducks !</h1>
</main>


<!-- css -->
<style>

</style>