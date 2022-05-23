<template>
  <div id="app">
    <Game/>
  </div>
</template>
<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import Game from './components/Game.vue'

@Component({
  components: { Game }
})
export default class App extends Vue {

}
</script>

<style src="./App.css">
</style>