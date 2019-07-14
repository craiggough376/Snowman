<template>
  <div id="app">
    <p>{{this.word}}</p>
    <button v-on:click="hideWord">Start Game</button>
    <p>{{this.hiddenWord}}</p>
    <letter-buttons></letter-buttons>
  </div>
</template>

<script>
import LetterButtons from "@/components/LetterButtons.vue"
import {eventBus} from './main.js'

export default {
  name: 'app',
  components: {
    "letter-buttons": LetterButtons
  },
  data(){
    return{
      word: "television",
      hiddenWord: null,
      chosenLetter: null
    }
  },
  methods: {
    hideWord(){
      let array = this.word.split("")
      this.hiddenWord = array
      for (let i = 0; i < array.length; i++){
          array[i] = "_"
      }
    }
  },
  mounted(){
    eventBus.$on('chosen-letter', (letter) => {
    this.chosenLetter = letter
  console.log(this.chosenLetter)})
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
