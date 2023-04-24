<template>
  <div :class="'row' + (isInvalid ? ' invalid' : '')" >
    <Tile v-for="i in 5" :letter="letters[i - 1]" :class="styling[i - 1]" />
  </div>
</template>

<script>
import Tile from './Tile.vue'

export default {
  props: ['letters', 'styling', 'isInvalid'],
  components: { Tile }
}
</script>

<style>
  .row {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    margin: auto;
    gap: 0.5rem;
  }
  .tile {
    position: relative;
    padding: 1rem;
    height: 1.625rem;
    width: 1.625rem;
    border: 1px solid black;
    font-size: 1.5rem;
    font-weight: bold;
  }
  .tile > * {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .invalid {
    color: red;
  }
  .invalid .tile {
    border: 1px solid red;
  }
</style>