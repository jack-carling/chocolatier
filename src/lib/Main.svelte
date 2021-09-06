<script lang="ts">
  import Calculator from './Calculator.svelte';
  import Graph from './Graph.svelte';
</script>

<main>
  <Calculator />
  <Graph />
</main>

<style>
  main {
    display: grid;
    gap: 2rem;
    grid-template-columns: 1fr 1fr;
  }
  @media screen and (max-width: 800px) {
    main {
      grid-template-columns: 1fr;
    }
  }
</style>
