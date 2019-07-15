<template>
  <div id="app">
    <button v-on:click="hideWord">Start Game</button>
    <p>{{displayWord}}</p>
    <p>Lives: {{this.lives}}</p>
    <letter-buttons v-if="this.start !== false"></letter-buttons>
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
      start: false,
      word: "shaun",
      hiddenWord: null,
      chosenLetter: null,
      lives: 5
    }
  },
  computed: {
    displayWord(){
      if (this.hiddenWord){
        return this.hiddenWord.join()
      }
    }
  },
  methods: {
    hideWord(){
      if (this.start === false){
        this.start = true}
      let array = this.word.split("")
      this.hiddenWord = array
      for (let i = 0; i < array.length; i++){
          array[i] = "_"
      }
    },
    checkLetter(){
      this.takeLife()
      let splitWord = this.word.split("")
      if (this.chosenLetter !== null){
        for(let i=0; i < splitWord.length; i++){
          if (splitWord[i] === this.chosenLetter){
            this.showLetter(splitWord[i]);
          }
        }
      }
    },
    showLetter(word){
      //search for letter in hiddenword
      const index = this.word.indexOf(word)
      console.log(index);
      this.hiddenWord.splice(index, 1)
      this.hiddenWord.splice(index, 0, word)
    },
    getChosenLetter(){
      eventBus.$on('chosen-letter', (letter) => {
      this.chosenLetter = letter
       // console.log(this.chosenLetter);
          this.checkLetter()})
      },

    takeLife(){
      this.lives -= 1
    }
  },

  mounted() {
    this.getChosenLetter()
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
