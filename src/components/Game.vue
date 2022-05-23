<template>
  <div class="game">
    <div class="game-board">
      <Board :squares="squares" :marked="marked" @click="handleClick" />
    </div>
    <div class="game-info">
      <Info :game="game" @clickJump="jumpTo" /> 
      <!--
      <div v-if="winner == null && draw == false" class="status">
        Next player: {{ xIsNext ? "X" : "O" }}
      </div>
      <div v-else-if="draw == false" class="status">Winner: {{ winner }}</div>
      <div v-else class="status">Draw!</div>
      <ol>
        <li v-for="(squares, i) of history" :key="i">
          <button @click="jumpTo(i)">Go to move {{ i }} ({{history[i].latestChange[0]+1}},{{history[i].latestChange[1]+1}})</button>
        </li>
      </ol> -->
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";

import Board from "./Board.vue";
import Info from "./Info.vue"

@Component({
  components: { Board, Info },
})
export default class Game extends Vue {
  history: boardData[] = [{squares: Array(9).fill(null), latestChange: []}];

  //
  marked: (boolean)[] = Array(9).fill(false); //4.

  stepNumber: number = 0;

  get squares(): ('X' | 'O' | null)[] { //3.
    if(this.history[this.stepNumber] != null){
      return this.history[this.stepNumber].squares;
    }
    else{
      return Array(9).fill(null);
    }
  } 

  jumpTo(i: number) {
    this.stepNumber = i;
    for(let i = 0; i<9; i++){
      this.marked[i] = false;
    }
  }

  get game(): gameState{
    return {winner: this.winner, draw: this.draw, xIsNext:this.xIsNext, history:this.history}
  }

  get xIsNext(): boolean {
    return this.stepNumber % 2 === 0;
  }

  get winner() {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (
        this.squares[a] &&
        this.squares[a] === this.squares[b] &&
        this.squares[a] === this.squares[c]
      ) {
        this.marked[a] = true; //4.
        this.marked[b] = true;
        this.marked[c] = true;

        return this.squares[a];
      }
    }
    return null;
  }

  get draw(){ //5.
    if(this.stepNumber==9){ 
      return true;
    }
    else return false;
  }

  getColumAndRow(i: number){
    let x = i % 3;
    let y = Math.floor( i / 3);

    return [x,y];
  }

  handleClick(i: number) {

    if (this.winner != null) return;

    let squares = this.squares.slice();
    //1. returns if you try to override square
    if(squares[i] != null) return;
    squares[i] = this.xIsNext ? "X" : "O";
    this.history = this.history.slice(0,this.stepNumber + 1);
    let state = {squares: squares, latestChange: this.getColumAndRow(i) };
    this.history.push(state);
    this.stepNumber++;
  }


}
interface boardData {
    squares: ('X' | 'O' | null)[];
    latestChange: (number)[]; // 0. -> x, 1. -> y
}

interface gameState {
  history: boardData[];
  winner: ('X' | 'O' | null);
  xIsNext: boolean;
  draw: boolean;
}

</script>