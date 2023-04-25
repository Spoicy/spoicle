<template>
  <div class="keyboard">
    <Key v-for="key in keys" :letter="key.letter" :size="key.size" :styling="usedLetters[key.letter]" @click="$emit('keyPress', '', key.letter)"/>
  </div>
</template>

<script>
import Key from './Key.vue'

export default {
  props: ['usedLetters'],
  emits: ['keyPress'],
  data() {
    return {
      // Redo with something more elegant maybe
      keys: [
        { letter: 'Q', size: 2 },
        { letter: 'W', size: 2 },
        { letter: 'E', size: 2 },
        { letter: 'R', size: 2 },
        { letter: 'T', size: 2 },
        { letter: 'Y', size: 2 },
        { letter: 'U', size: 2 },
        { letter: 'I', size: 2 },
        { letter: 'O', size: 2 },
        { letter: 'P', size: 2 },
        { letter: '', size: 1 },
        { letter: 'A', size: 2 },
        { letter: 'S', size: 2 },
        { letter: 'D', size: 2 },
        { letter: 'F', size: 2 },
        { letter: 'G', size: 2 },
        { letter: 'H', size: 2 },
        { letter: 'J', size: 2 },
        { letter: 'K', size: 2 },
        { letter: 'L', size: 2 },
        { letter: '', size: 1 },
        { letter: 'Back', size: 3 },
        { letter: 'Z', size: 2 },
        { letter: 'X', size: 2 },
        { letter: 'C', size: 2 },
        { letter: 'V', size: 2 },
        { letter: 'B', size: 2 },
        { letter: 'N', size: 2 },
        { letter: 'M', size: 2 },
        { letter: 'Enter', size: 3 },
      ]
    }
  },
  components: { Key },
  methods: {
    testMethod() {
      console.log("test")
    }
  },
}
</script>

<style>
  .keyboard {
    width: 400px;
    margin-top: 1rem;
    margin: auto;
    padding: 0.5rem;
    display: grid;
    gap: 0.125rem;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(20, 1fr);
  }
</style>