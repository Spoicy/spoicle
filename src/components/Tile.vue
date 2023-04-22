<template>
  <div class="tile">
    {{ letter }}
  </div>
</template>

<script>
  export default {
    props: ['letter']
  }
</script>

<style>
  .tile {
    position: relative;
    padding: 1rem;
    height: 1.625rem;
    width: 1.625rem;
    border: 1px solid black;
    font-size: 1.5rem;
    font-weight: bold;
    background-color: #fafafa;
  }
  .tile > * {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  .tile.green {
    background-color: #0c8;
  }
  .tile.yellow {
    background-color: #fc0;
  }
  .tile.gray {
    background-color: #ddd;
  }
</style>