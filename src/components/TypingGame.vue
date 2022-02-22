<template>
  <header class="text-center p-3 mb-5">
    <h1>Typing Game</h1>
  </header>
  <div class="container text-center">
    <div class="row">
      <div class="col-md-6 mx-auto">
        <h2 v-if="gameOver" class="display-2 mb-5">Game Over!</h2>
        <h2 v-else class="display-2 mb-5">
          <span
            v-for="(item, index) in word"
            :key="index"
            :class="[item.active ? 'valid-letter' : '', hidden ? 'hidden' : '']"
          >
            {{ item.letter }}
          </span>
        </h2>

        <input
          type="text"
          class="form-control form-control-lg"
          placeholder="Start typing..."
          autofocus
          v-model="input"
        />
        <div class="progress">
          <div class="progress-bar" role="progressbar" :style="increaseBar"></div>
        </div>
        <h4 class="mt-3" id="message"></h4>
        <br />
        <button :disabled="isActive" @click="start()" class="btn btn-lg btn-block">START</button>

        <div class="row mt-3">
          <div class="col-md-6">
            <h3>
              Time Left:
              <span id="time">{{ time }}</span>
            </h3>
          </div>
          <div class="col-md-6">
            <h3>
              Score:
              <span id="score"> {{ score }} </span>
            </h3>
          </div>
          <div class="col mt-3 d-flex justify-content-evenly">
            <h3>Difficulty:</h3>
            <select class="input-level" v-model="selected" :disabled="isActive">
              <option v-for="option in options" :value="option" :key="option">
                {{ option.level }}
              </option>
            </select>
          </div>
          <div class="col mt-3">
            <h3>
              High Score:
              <span>{{ highScore }}</span>
            </h3>
          </div>
        </div>

        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card card-body text-white">
              <h5 class="text-warning">Instructions</h5>
              <p>Type as many words as you can until time runs out</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { WORDS_LIST } from '../utils/wordList'

export default {
  name: 'TypingGame',
  data() {
    return {
      isActive: false,
      hidden: false,
      gameOver: false,
      time: 30,
      word: [],
      input: '',
      checker: 0,
      score: 0,
      highScore: localStorage.highScore | 0,
      width: 0,
      selected: 'Easy',
      options: [
        { level: 'Easy', time: 30 },
        { level: 'Medium', time: 20 },
        { level: 'Hard', time: 10 }
      ]
    }
  },
  methods: {
    start() {
      this.isActive = true
      this.countDown()
      this.initWord()
    },
    countDown() {
      const timer = setInterval(() => {
        this.time--
        if (this.time === 0) {
          clearInterval(timer)
          this.time = this.selected.time
          this.gameOver = true
          this.isActive = false
          if (this.score > this.highScore) {
            localStorage.highScore = this.score
            this.highScore = this.score
          }
        }
      }, 1000)
    },
    initWord() {
      const active = false

      const word = WORDS_LIST[Math.floor(Math.random() * WORDS_LIST.length)].toUpperCase()

      this.word = word.split('').map(letter => {
        return { letter, active }
      })
    },
    increaseWidth() {
      this.width += 10
    }
  },
  watch: {
    input(value) {
      const currentWord = value.toUpperCase().split('')

      const lastLetter = currentWord[currentWord.length - 1]

      const letterToFind = this.word.find(letter => !letter.active)

      if (letterToFind.letter === lastLetter) {
        this.word = [...this.word, (letterToFind.active = true)]
        this.checker++
      }

      if (this.checker === 5) {
        this.hidden = true

        setTimeout(() => {
          this.score++
          this.hidden = false
          this.input = ''
          this.checker = 0
          this.increaseWidth()
          this.initWord()
        }, 500)
      }
    },
    selected(value) {
      this.time = value.time
    }
  },
  computed: {
    increaseBar() {
      return {
        width: `${this.width}%`
      }
    }
  }
}
</script>

<style scoped>
header,
.card {
  background: black;
}

.form-control:focus {
  border-color: #e9f897;
  box-shadow: 0 0 0 0.2rem rgba(123, 167, 40, 0.25);
}

.card > p,
header {
  color: white;
}

#message {
  height: 26px;
}

.hidden {
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s 1s, opacity 1s linear;
}

.valid-letter {
  background-color: rgb(223, 223, 55);
  color: rgb(24, 24, 24);
}

.btn,
.input-level {
  background: #f4e623;
}

.input-level {
  border-radius: 0.25rem;
}

.progress {
  margin-top: 22px;
  height: 2rem;
  border-radius: 0.25rem;
}

.progress-bar {
  background-color: #e9f897;
  background-image: linear-gradient(to bottom, #e9f897, #f4e623);
}
</style>
