<template>
  <div :class="'key ' + styling +  ' size-' + size + (letter === '' ? ' nostyle' : ' ')">
    <button v-if="letter !== ''">{{ letter }}</button>
  </div>
</template>

<script>
export default {
  props: ['letter', 'size', 'styling']
}
</script>

<style>
  .key button {
    width: 100%;
    aspect-ratio: 1;
    border: 1px solid black;
  }
  .key.size-3 button {
    aspect-ratio: 3 / 2;
  }
  .size-2 {
    grid-column: span 2;
  }
  .size-3 {
    grid-column: span 3;
  }
  .key.white button {
    background-color: white;
  }
  .key.green button {
    background-color: #0c8;
  }
  .key.yellow button {
    background-color: #fc0;
  }
  .key.gray button {
    background-color: #ddd;
  }
</style>
<!-- (letter === '' ? 'nostyle' : '') + ' size-' + size" -->