<script lang="ts">
import {defineComponent} from 'vue'
export default defineComponent({
  data(){
    return{
      colors: this.makeColorsList(),
      isCorrect: undefined as undefined | boolean,
    }
  },

  computed: {
    correctGuess(){
      return this.colors[Math.floor(Math.random() * this.colors.length)]
    }
  },

  methods:{
    handleGuess(guess: string){
      if(guess === this.correctGuess){
        this.isCorrect = true
        this.colors = this.makeColorsList()
        return
      }

      this.isCorrect = false
    },
    generateColor(){
      const hexChars = "0123456789ABCDEF";
      const getChar = () => hexChars.charAt(Math.floor(Math.random() * hexChars.length))

      let generatedColor = "#"

      for (let i = 0; i < 6; i++) {
        generatedColor += getChar();
      }

      return generatedColor;
    },
     makeColorsList(){
      let colors = []
      for (let i = 0; i < 3; i++) {
        colors.push(this.generateColor())
      }

      return colors;
    }
  },

})
</script>

<template>
  <div class="guess-me" :style="{ backgroundColor: correctGuess }" />
  <div class="guesses">
    <button v-for="color in colors" key="color" @click="handleGuess(color)">{{ color }}</button>
  </div>
  <h3 v-if="isCorrect === undefined">Make a guess</h3>
  <h3 v-else-if="isCorrect" class="correct">Correct</h3>
  <h3 v-else class="incorrect">Incorrect</h3>
</template>

<style scoped>
.guess-me {
  height: 200px;
  width: 220px;
}

button {
  margin-top: 10px;
}

button ~ button {
  margin-left: 10px;
}

.correct {
  color: green
}

.incorrect {
  color: red
}

h3 {
  text-align: center;
}
</style>
