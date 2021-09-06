<script lang="ts">
  import Welcome from './lib/Welcome.svelte';
  import Main from './lib/Main.svelte';

  let showWelcome = true;

  function hideWelcome() {
    showWelcome = false;
  }
</script>

<main>
  <h1>Chocolatier</h1>
  {#if showWelcome}
    <Welcome on:click={hideWelcome} />
  {:else}
    <Main />
  {/if}
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@100;400&display=swap');

  :root {
    background-color: #ffe8d6;
    color: #7f5539;
    font-family: 'Josefin Sans', sans-serif;
    font-weight: 100;
  }
  main {
    padding: 2rem;
  }
  h1 {
    text-align: center;
    text-transform: uppercase;
    font-size: 4rem;
    font-weight: 100;
    margin: 2rem 0;
  }

  @media screen and (max-width: 600px) {
    h1 {
      font-size: 2rem;
    }
  }
</style>
