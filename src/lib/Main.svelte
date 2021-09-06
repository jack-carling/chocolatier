<script lang="ts">
  import Calculator from './Calculator.svelte';
  import Graph from './Graph.svelte';

  import type { IngredientsData } from '../utils/interfaces';

  let ingredients: IngredientsData[] = [];

  function setIngredients(event: CustomEvent) {
    ingredients = event.detail;
  }
</script>

<main>
  <Calculator on:ingredients={setIngredients} />
  <Graph {ingredients} />
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
