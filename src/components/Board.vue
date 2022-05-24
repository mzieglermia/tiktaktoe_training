<template>
  <div>
    <!-- tried a table:-->
    <tbody>
      <tr v-for="i1 in 3" :key="i1 - 1">
        <td v-for="i2 in 3" :key="i2 - 1">
          <Square
            :value="squares[index(i1,i2)]"
            :marked="marked[index(i1,i2)]"
            @click="$emit('click', index(i1,i2))"
          />
        </td>
      </tr>
    </tbody>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";
import { Prop } from "vue-property-decorator";

import Square from "./Square.vue";

@Component({
  components: { Square },
})
export default class Board extends Vue {
  @Prop({}) squares;
  @Prop({}) marked; //4.
  width: number = 3;

  //calculate the index number for the square out of the position
  index(i1,i2):number{
    return (i1 - 1) * 3 + (i2 - 1);
  }

}

</script>