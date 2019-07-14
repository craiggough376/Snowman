<template>
  <div id="app">
    <p>{{this.word}}</p>
    <button v-on:click="hideWord">Start Game</button>
    <p>{{this.hiddenWord}}</p>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {

  },
  data(){
    return{
      word: "television",
      hiddenWord: null
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
