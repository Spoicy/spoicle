<template>
  <div>
    <div class="rows">
      <Row v-for="i in 6" :letters="words[i-1].split('')" :styling="styling[i-1]" :isInvalid="((tryCount + 1 === i) ? wordInvalid : '')" />
    </div>
    <p v-if="tryCount >= 6">The word was: {{ answerWord }}</p>
    <button class="restartButton" @click="restartGame">New game</button>
    <Keyboard :usedLetters="usedLetters" @keyPress="updateWord"/>
  </div>
</template>

<script>
import Row from './Row.vue'
import Keyboard from './Keyboard.vue'

export default {
  components: { Row, Keyboard },
  data() {
    return {
      answerWord: '',
      tryCount: 0,
      words: ['', '', '', '', '', ''],
      styling: [[], [],  [], [], [], []],
      letterCounts: {},
      wordList: null,
      wordInvalid: false,
      usedLetters: [],
      solved: false
    }
  },
  created() {
    fetch('https://gist.githubusercontent.com/dracos/dd0668f281e685bad51479e5acaadb93/raw/6bfa15d263d6d5b63840a8e5b64e04b382fdb079/valid-wordle-words.txt')
      .then(response => response.text())
      .then(data => {
        this.wordList = data.split('\n');
        this.wordList = this.wordList.map(function (x) { return x.toUpperCase(); })
        this.answerWord = this.wordList[Math.floor(Math.random() * this.wordList.length)];
      });
    for (let i = 65; i <= 90; i++) {
      this.usedLetters[String.fromCharCode(i)] = 'white'
    }
  },
  mounted() {
    document.addEventListener('keyup', this.updateWord);
  },
  beforeUnmount() {
    document.removeEventListener('keyup', this.updateWord);
  },
  methods: {
    updateWord(e, pressedKey) {
      if (pressedKey) {
        console.log('test');
        e = { key: pressedKey };
      }
      if (this.solved || this.tryCount >= 6) {
        return;
      }
      if (e.key === 'Backspace' && this.words[this.tryCount]) {
        this.words[this.tryCount] = this.words[this.tryCount].slice(0, -1);
      } else if (/^[a-zA-Z]$/.test(e.key) && this.words[this.tryCount].length < 5) {
        this.words[this.tryCount] += e.key.toUpperCase();
      } else if (e.key === 'Enter' && this.words[this.tryCount].length === 5) {
        if (this.wordList.includes(this.words[this.tryCount])) {
          this.checkWord(this.words[this.tryCount]);
          this.tryCount++;
        } else {
          this.wordInvalid = true;
          setTimeout(() => {
            this.wordInvalid = false;
          }, 1000)
        }
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
      if (this.words[this.tryCount] === this.answerWord) {
        this.solved = true;
        for (let i = 0; i < 5; i++) {
          this.styling[this.tryCount][i] = 'green';
          this.markKey(this.words[this.tryCount][i], 'green');
        }
      } else {
        for (let i = 0; i < 5; i++) {
          if (this.words[this.tryCount].substring(i, i+1) == this.answerWord.substring(i, i+1)) {
            this.styling[this.tryCount][i] = 'green';
            this.markKey(this.words[this.tryCount][i], 'green');
            this.letterCounts[this.words[this.tryCount][i]]--;
          }
        }
        for (let i = 0; i < 5; i++) {
          if (this.answerWord.includes(this.words[this.tryCount][i]) && !this.styling[this.tryCount][i]
              && this.letterCounts[this.words[this.tryCount][i]] > 0) {
            this.styling[this.tryCount][i] = 'yellow';
            this.markKey(this.words[this.tryCount][i], 'yellow');
            this.letterCounts[this.words[this.tryCount][i]]--;
          } else if (!this.styling[this.tryCount][i]) {
            this.styling[this.tryCount][i] = 'gray'
            this.markKey(this.words[this.tryCount][i], 'gray');
          }
        }
      }
      console.log(this.letterCounts);
      this.letterCounts = {};
    },
    restartGame() {
      this.answerWord = this.answerWord = this.wordList[Math.floor(Math.random() * this.wordList.length)];
      this.tryCount = 0;
      this.words = ['', '', '', '', '', ''];
      this.styling = [[], [], [], [], [], []];
      this.letterCounts = {};
      for (let i = 65; i <= 90; i++) {
        this.usedLetters[String.fromCharCode(i)] = 'white';
      }
      this.solved = false;
    },
    markKey(key, state) {
      if (this.usedLetters[key] == 'green' || (this.usedLetters[key] !== 'gray' && state === 'gray') || state === 'white') {
        return;
      }
      this.usedLetters[key] = state;
    },
    testMethod(e, pressedKey) {
      console.log(e, pressedKey);
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