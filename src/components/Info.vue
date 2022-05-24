<template>
  <div>
    <div v-if="game.winner == null && game.draw == false" class="status">
      Next player: {{ game.xIsNext ? "X" : "O" }}
    </div>
    <div v-else-if="game.draw == false" class="status">
      Winner: {{ game.winner }}
    </div>
    <div v-else class="status">
      Draw! <!-- 5. -->
    </div> 

    <ol>
      <li v-for="(squares, i) of game.history" :key="i">
        <button @click="$emit('clickJump',i)" :class="`listbutton ${isBold(i)}`">Go to move {{ i }} ({{game.history[i].latestChange[0]+1}},{{game.history[i].latestChange[1]+1}})</button>
      </li>
    </ol>
  </div>
</template>

<script lang="ts">

import Vue from "vue";
import boardData from "Game.vue";
import gameState from "Game.vue";
import Component from "vue-class-component";

import { Prop } from "vue-property-decorator";

@Component({})
export default class Info extends Vue {
  @Prop({}) game: gameState;

  isBold(i:number): boolean{
    if(i==this.game.stepNumber) return true;
    else return false;
  }
}
</script>