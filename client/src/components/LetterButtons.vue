<template lang="html">
  <div>
    <p>Choose a letter (Hint: film)</p>
    <button v-on:click="chooseLetter(letter)" v-for="letter in alphabet.split('')" onclick="style='display: none;'">{{letter}}</button>
  </div>
</template>


<script>
import { eventBus } from '../main.js'
export default {
  name: 'letter-buttons',
  data(){
    return{
      alphabet: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    }
  },
  methods: {
    chooseLetter(letter){
      eventBus.$emit('chosen-letter', letter)
    }
  }
}
</script>

<style lang="css" scoped>

</style>
