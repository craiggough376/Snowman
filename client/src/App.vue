<template>
  <div id="app">
    <p>{{displayWord}}</p>
    <p v-if="this.start !== false">Lives: {{this.lives}}
    <p v-if="this.won === true"><span style='font-size:30px;'>&#9786;</span> CORRECT! You win!</p>
    <p v-if="this.lost === true"><span style='font-size:30px;'>&#9785;</span> You Lose</p>
    <letter-buttons v-if="this.start !== false"></letter-buttons>
    <br>
    <div v-if="this.start === false && ((this.won === false) && (this.lost === false))" class="">
      <h1>SNOWMAN<span style='font-size:40px;'>&#9924;</span></h1>
      <button v-on:click="startGame">Start Game</button>
    </div>
    <button v-if="this.start === true || ((this.won === true) || (this.lost === true))" v-on:click="startGame">Restart Game</button>


  </div>
</template>

<!--
todo list
2 get word from larger array(another vue?)
3 be able to have multiple word answers
4 add html hangman
5 add css
-->

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
      words: ["GOODFELLAS", "SCARFACE", "JAWS", "ET", "BABE", "ALIEN", "ALADDIN", "ROCKY", "PSYCHO", "CASABLANCA", "ELF"],
      word: null,
      holdWord: null,
      hiddenWord: null,
      chosenLetter: null,
      lives: null,
      won: false,
      lost: false
    }
  },
  computed: {
    displayWord(){
      if (this.hiddenWord && this.start !== false){
        return this.hiddenWord.join(" ")
      }else{
        if (this.holdWord){
          let array = this.holdWord.split("")
        return array.join(" ")
        }
      }
    }
  },
  methods: {
    startGame(){
      //hides word and resets values
      this.getWord()
      if (this.start === false){
        this.start = true}
      let array = this.word.split("")
      this.hiddenWord = array
      for (let i = 0; i < array.length; i++){
          array[i] = "_"
          this.won = false;
          this.lost = false;
          this.lives = 6;
      }
    },
    getWord(){
      this.word = this.words[Math.floor(Math.random() * this.words.length)]
      let index = this.words.indexOf(this.word)
      this.words.splice(index, 1)
      this.holdWord = this.word
      console.log(this.word);
      console.log(this.words);
    },
    getChosenLetter(){
      eventBus.$on('chosen-letter', (letter) => {
      this.chosenLetter = letter
       // console.log(this.chosenLetter);
          this.checkLetter()})
      },
      checkLives(){
        if(this.lives >= 1){
          return true
        }else{
          this.endGame();
          this.lost = true;
        }
      },
    checkLetter(){
      if(this.checkLives() === true){
        this.takeLife();
        let splitWord = this.word.split("")
          if (this.chosenLetter !== null){
            for(let i=0; i < splitWord.length; i++){
              if (splitWord[i] === this.chosenLetter){
                this.showLetter(splitWord[i]);
              }
            }
          }
        }
    },
    takeLife(){
      let splitWord = this.word.split("")
      // console.log(splitWord);
      // console.log(this.chosenLetter);
      if (splitWord.includes(this.chosenLetter)){
        return true;
      }else{
        this.lives -= 1;
        this.checkWon();
      }
    },
    showLetter(letter){
      //search for letter in hiddenword
          const index = this.word.indexOf(letter)
          this.hiddenWord.splice(index, 1)
          this.hiddenWord.splice(index, 0, letter)
          let splitWord = this.word.split("")
          splitWord.splice(index, 1)
          splitWord.splice(index, 0, "_")
          this.word = splitWord.join("")
          this.checkWon()
          // console.log(this.word);
    },
    checkWon(){
      let array = this.word.split("")
      if((array.every(letter => letter === "_") || this.lives === 0)){
        this.endGame()
      }
    },
    endGame(){
        this.start = false;
      if(this.lives >= 1){
        this.won = true
      }else{
            this.lost = true;
        }
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
