<script lang="ts">
  import data from '../utils/data';
  import type { IngredientsData } from '../utils/interfaces';
  import { createEventDispatcher, tick, onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  let ingredients: IngredientsData[] = [];

  let newIngredient = '';

  $: totalAmount = ingredients.reduce((total, ingredient) => {
    return ingredient.amount + total;
  }, 0);

  $: percentages = ingredients.map((ingredient) => {
    const percent = (ingredient.amount / totalAmount) * 100;
    if (isNaN(percent)) return '0.00';
    return percent.toFixed(2);
  });

  function setAmount(amount: number) {
    if (isNaN(Number(amount))) {
      return 0;
    } else {
      return Number(amount);
    }
  }

  function addIngredient() {
    if (!newIngredient) return;
    let ingredient: IngredientsData;
    for (let category in data) {
      ingredient = data[category].find((i: IngredientsData) => i.name === newIngredient);
      if (ingredient) break;
    }
    ingredients = [...ingredients, { ...ingredient, amount: 0 }];
  }

  function removeIngredient(index: number) {
    ingredients.splice(index, 1);
    ingredients = ingredients;
  }

  async function dispatchIngredients() {
    await tick();
    const payload = ingredients.map((ingredient, index) => {
      return { ...ingredient, percentage: percentages[index] };
    });
    dispatch('ingredients', payload);
  }

  onMount(() => {
    let ingredient: IngredientsData;
    ingredient = data.dairy.find((i: IngredientsData) => i.name === 'Cream 36%');
    ingredients = [...ingredients, { ...ingredient, amount: 350 }];
    ingredient = data.sugars.find((i: IngredientsData) => i.name === 'Glucose');
    ingredients = [...ingredients, { ...ingredient, amount: 115 }];
    ingredient = data.sugars.find((i: IngredientsData) => i.name === 'Inverted sugar');
    ingredients = [...ingredients, { ...ingredient, amount: 115 }];
    ingredient = data.fats.find((i: IngredientsData) => i.name === 'Clarified butter');
    ingredients = [...ingredients, { ...ingredient, amount: 50 }];
    ingredient = data.chocolate.find((i: IngredientsData) => i.name === 'Callebaut 811 - 54,5%');
    ingredients = [...ingredients, { ...ingredient, amount: 650 }];
    ingredient = data.fats.find((i: IngredientsData) => i.name === 'Cocoa butter');
    ingredients = [...ingredients, { ...ingredient, amount: 20 }];
    dispatchIngredients();
  });
</script>

<section>
  <div class="calculator-menu">
    <span>Add ingredient:</span>
    <div class="select">
      <i class="material-icons">expand_more</i>
      <select name="" id="" bind:value={newIngredient}>
        <optgroup label="Dairy">
          {#each data.dairy as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
        <optgroup label="Fats">
          {#each data.fats as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
        <optgroup label="Sugars">
          {#each data.sugars as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
        <optgroup label="Fruit purées">
          {#each data.fruits as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
        <optgroup label="Chocolates">
          {#each data.chocolate as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
        <optgroup label="Alcohols">
          {#each data.alcohol as { name }}
            <option value={name}>{name}</option>
          {/each}
        </optgroup>
      </select>
    </div>
    <i class="material-icons" on:click={addIngredient}>add_circle_outline</i>
  </div>
  {#if ingredients.length}
    <table>
      <tr>
        <td class="header">Ingredient</td>
        <td class="header">Amount (g)</td>
        <td class="header">%</td>
        <td class="header">Remove</td>
      </tr>
      {#each ingredients as { name, amount }, i}
        <tr>
          <td>{name}</td>
          <td>
            <input
              maxlength="7"
              type="text"
              bind:value={amount}
              on:input={() => {
                amount = setAmount(amount);
                dispatchIngredients();
              }}
            />
          </td>
          <td>
            {percentages[i]}
          </td>
          <td class="remove">
            <div>
              <i
                class="material-icons"
                on:click={() => {
                  removeIngredient(i);
                  dispatchIngredients();
                }}>remove_circle_outline</i
              >
            </div>
          </td>
        </tr>
      {/each}
      <tr>
        <td class="header">Total</td>
        <td>{totalAmount}</td>
        <td>100.00</td>
        <td />
      </tr>
    </table>
  {/if}
</section>

<style>
  table {
    width: 100%;
  }
  table,
  tr,
  td {
    border: 1px solid #7f5539;
    border-collapse: collapse;
  }
  td {
    padding: 0.3rem;
  }
  td.header {
    font-weight: 400;
  }
  td.remove div {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  td.remove i {
    font-size: 20px;
  }
  input {
    appearance: none;
    border: none;
    outline: none;
    width: 100%;
    color: #7f5539;
    font-family: 'Josefin Sans', sans-serif;
    font-weight: 100;
    font-size: 1rem;
    padding: 0.2rem;
    background-color: rgba(255, 255, 255, 0.5);
  }
  div.calculator-menu {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
  }
  div.calculator-menu i {
    font-size: 20px;
  }
  select {
    cursor: pointer;
    appearance: none;
    border: none;
    outline: none;
    color: #7f5539;
    font-family: 'Josefin Sans', sans-serif;
    font-weight: 100;
    font-size: 1rem;
    padding: 0.2rem;
    padding-right: 1rem;
  }
  div.select {
    margin: 0 0.5rem;
    position: relative;
  }
  div.select i {
    position: absolute;
    right: 0;
    height: 20px;
    top: 0;
    bottom: 0;
    margin: auto 0;
    pointer-events: none;
  }
</style>
