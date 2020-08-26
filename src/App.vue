<template>
  <div id="app">
    <div v-if="gameStart==false">
      <button @click="start(),randomplayer(0,3),starts()" class="btn btn-danger">Start</button>
    </div>

    <div class="col-md-6">
      <game :gameStart="gameStart" :you="you" ref="gm" />
    </div>
  </div>
</template>

<script>
import game from "./components/game.vue";

export default {
  name: "App",

  components: {
    game,
  },
  data: function () {
    return {
      gameStart: false,
      you: 0,
    };
  },
  methods: {
    start() {
      this.gameStart = true;
    },
    randomplayer: function (min, max) {
      this.you = Math.max(Math.floor(Math.random() * max) + 1, min);
      return this.you;
    
      
    },
    starts() {
      this.$refs.gm.starthp()

    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>