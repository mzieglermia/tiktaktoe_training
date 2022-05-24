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
    <button @click="reverse()">Reverse the list</button>
    <ol>
      <li v-for="(squares, i) of historySorted" :key="i">
        <button @click="$emit('clickJump',historySorted[i].stepNumber)" 
        :class="`listbutton ${isBold(historySorted[i].stepNumber)}`">
          Go to move {{ historySorted[i].stepNumber }} ({{historySorted[i].latestChange[0]+1}},{{historySorted[i].latestChange[1]+1}})
        </button>
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
  //indicates whether the list is reversed or not.
  reversed: boolean = false;

  //bolds the current selected board state in the info box.
  isBold(i:number): boolean{
    if(i==this.game.currentStepNumber)
    {
      return true;
    }
    else return false;
  }
  reverse(){
    this.reversed=!this.reversed;
  }
  //returns reversed history if button is pressed.
  get historySorted(){
    if(this.reversed) return [...this.game.history].reverse();
    else return this.game.history;
  }
}
</script>