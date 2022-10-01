<script setup lang="ts">

const generateColor = () => {
  const hexChars = "0123456789ABCDEF";
  const getChar = () => hexChars.charAt(Math.floor(Math.random() * hexChars.length))

  let generatedColor = "#"

  for (let i = 0; i < 6; i++) {
    generatedColor += getChar();
  }

  return generatedColor;
}

const makeColorsList = () => {
  let colors = []
  for (let i = 0; i < 3; i++) {
    colors.push(generateColor())
  }

  return colors;
}

let colors = $ref(makeColorsList())
let isCorrect = $ref<undefined | boolean>()
let correctGuess = $computed(() => colors[Math.floor(Math.random() * 3)] )

const handleGuess = (guess: string) => {
  if(guess === correctGuess){
    isCorrect = true
    colors = makeColorsList()
    return
  }

  isCorrect = false

}

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
