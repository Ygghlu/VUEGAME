/* eslint-disable no-unused-vars */
<template>
  <div id="app" class="w-100 h-100 p-3 mx-auto flex-column">
    <game />
  </div>
</template>

<script>
import Vue from "vue";
import { BootstrapVue, BootstrapVueIcons } from "bootstrap-vue";
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap-vue/dist/bootstrap-vue.css";
import "./assets/css/cover.css";
import VueCarousel from "vue-carousel";
Vue.use(VueCarousel);
Vue.use(BootstrapVue);
Vue.use(BootstrapVueIcons);

import game from "./components/game.vue";

export default {
  name: "App",

  components: {
    game,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 64px;
}
</style>