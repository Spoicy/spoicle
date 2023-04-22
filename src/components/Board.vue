<template>
  <div>
    <div class="rows">
      <Row v-for="i in 6" :letters="words[i-1].split('')" :styling="styling[i-1]" />
    </div>
    <p v-if="tryCount >= 6">The word was: {{ answerWord }}</p>
    <button class="restartButton" @click="restartGame">New game</button>
  </div>
</template>

<script>
import Row from './Row.vue'

export default {
  components: { Row },
  data() {
    return {
      answerWord: '',
      tryCount: 0,
      words: ['', '', '', '', '', ''],
      styling: [[], [],  [], [], [], []],
      letterCounts: {},
      wordList: null,
      solved: false
    }
  },
  created() {
    fetch('https://gist.githubusercontent.com/dracos/dd0668f281e685bad51479e5acaadb93/raw/6bfa15d263d6d5b63840a8e5b64e04b382fdb079/valid-wordle-words.txt')
      .then(response => response.text())
      .then(data => {
        this.wordList = data.split('\n');
        this.answerWord = this.wordList[Math.floor(Math.random() * this.wordList.length)].toUpperCase();
      });
  },
  mounted() {
    document.addEventListener('keyup', this.updateWord);
    
  },
  beforeUnmount() {
    document.removeEventListener('keyup', this.updateWord);
  },
  methods: {
    updateWord(e) {
      if (this.solved || this.tryCount >= 6) {
        return;
      }
      if (e.key === 'Backspace' && this.words[this.tryCount]) {
        this.words[this.tryCount] = this.words[this.tryCount].slice(0, -1);
      } else if (/^[a-zA-Z]$/.test(e.key) && this.words[this.tryCount].length < 5) {
        this.words[this.tryCount] += e.key.toUpperCase();
      } else if (e.key === 'Enter' && this.words[this.tryCount].length === 5) {
        this.checkWord(this.words[this.tryCount]);
        this.tryCount++;
      }
    },
    checkWord(word) {
      for (let i = 0; i < 5; i++) {
        if (!this.letterCounts[this.answerWord[i]]) {
          this.letterCounts[this.answerWord[i]] = 1;
        } else {
          this.letterCounts[this.answerWord[i]]++;
        }
      }
      if (this.words[this.tryCount] === this.answerWord.toUpperCase()) {
        this.solved = true;
        for (let i = 0; i < 5; i++) {
          this.styling[this.tryCount][i] = 'green';
        }
      } else {
        for (let i = 0; i < 5; i++) {
          if (this.words[this.tryCount].substring(i, i+1) == this.answerWord.toUpperCase().substring(i, i+1)) {
            this.styling[this.tryCount][i] = 'green';
            this.letterCounts[this.words[this.tryCount][i]]--;
          }
        }
        for (let i = 0; i < 5; i++) {
          if (this.answerWord.toUpperCase().includes(this.words[this.tryCount][i]) && !this.styling[this.tryCount][i]
              && this.letterCounts[this.words[this.tryCount][i]] > 0) {
            this.styling[this.tryCount][i] = 'yellow';
            this.letterCounts[this.words[this.tryCount][i]]--;
          } else if (!this.styling[this.tryCount][i]) {
            this.styling[this.tryCount][i] = 'gray'
          }
        }
      }
      console.log(this.letterCounts);
      this.letterCounts = {};
    },
    restartGame() {
      this.answerWord = this.answerWord = this.wordList[Math.floor(Math.random() * this.wordList.length)].toUpperCase();
      this.tryCount = 0;
      this.words = ['', '', '', '', '', ''];
      this.styling = [[], [], [], [], [], []];
      this.letterCounts = {};
      this.solved = false;
    }
  },
}
</script>

<style>
.rows {
  display: grid;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
.restartButton {
  font-size: 1.5rem;
}
</style>