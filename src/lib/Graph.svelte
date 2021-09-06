<script lang="ts">
  import type { IngredientsData } from '../utils/interfaces';
  export let ingredients: IngredientsData[] = [];

  $: water = ingredients
    .reduce((total, ingredient) => {
      return parseFloat(ingredient.percentage) * ingredient.waterContent + total;
    }, 0)
    .toFixed(2);
  $: sugar = ingredients
    .reduce((total, ingredient) => {
      return parseFloat(ingredient.percentage) * ingredient.sugarContent + total;
    }, 0)
    .toFixed(2);
  $: cocoaFat = ingredients
    .reduce((total, ingredient) => {
      return parseFloat(ingredient.percentage) * ingredient.cocoaFatContent + total;
    }, 0)
    .toFixed(2);
  $: dairyFat = ingredients
    .reduce((total, ingredient) => {
      return parseFloat(ingredient.percentage) * ingredient.dairyFatContent + total;
    }, 0)
    .toFixed(2);
  $: totalFat = (parseFloat(cocoaFat) + parseFloat(dairyFat)).toFixed(2);
</script>

<section>
  <h1>Total</h1>
  <span>Water (recommended 0-20%)</span>
  <div class="bar">
    <div class="safe-zone" id="water" />
    <div class="percentage" style="width: {water}%" />
  </div>
  <span>Total water: {water} %</span>
  <div class="line" />
  <span>Sugar (recommended minimum 25-30%)</span>
  <div class="bar">
    <div class="safe-zone" id="sugar" />
    <div class="percentage" style="width: {sugar}%" />
  </div>
  <span>Total sugar: {sugar} %</span>
  <div class="line" />
  <span>Cocoa butter (recommended 10-30%)</span>
  <div class="bar">
    <div class="safe-zone" id="cocoa-butter" />
    <div class="percentage" style="width: {cocoaFat}%" />
  </div>
  <span>Total cocoa butter: {cocoaFat} %</span>
  <div class="line" />
  <span>Dairy fat (recommended maximum 15%)</span>
  <div class="bar">
    <div class="safe-zone" id="dairy-fat" />
    <div class="percentage" style="width: {dairyFat}%" />
  </div>
  <span>Total dairy fat: {dairyFat} %</span>
  <div class="line" />
  <span>Fat content (recommended 25-40%)</span>
  <div class="bar">
    <div class="safe-zone" id="fat" />
    <div class="percentage" style="width: {totalFat}%" />
  </div>
  <span>Total fat: {totalFat} %</span>
</section>

<style>
  h1 {
    text-transform: uppercase;
    font-weight: 100;
    margin: 0;
    margin-bottom: 1rem;
  }
  span {
    display: block;
  }
  div.bar {
    margin: 0.5rem 0;
    border: 1px solid #7f5539;
    width: 100%;
    height: 30px;
    background: repeating-linear-gradient(45deg, #ffede0, #ffede0 10px, #ffe8d6 10px, #ffe8d6 20px);
    position: relative;
  }
  div.safe-zone {
    height: 100%;
    background-color: #a5daa1;
    opacity: 0.5;
    position: absolute;
    top: 0;
  }
  #water {
    width: 20%;
    left: 0;
  }
  #sugar {
    width: 5%;
    left: 25%;
  }
  #cocoa-butter {
    width: 20%;
    left: 10%;
  }
  #dairy-fat {
    width: 15%;
    left: 0;
  }
  #fat {
    width: 15%;
    left: 25%;
  }
  div.percentage {
    width: 0%;
    height: 10px;
    position: absolute;
    left: 0;
    top: 10px;
    background-color: #4bb543;
    transition: width 0.5s linear;
  }
  div.line {
    width: 100%;
    border-bottom: 1px solid #7f5539;
    margin: 0.5rem 0 1rem 0;
  }
</style>
