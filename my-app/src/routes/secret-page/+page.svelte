<script>
    import Banner from "$lib/Banner.svelte"; 
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