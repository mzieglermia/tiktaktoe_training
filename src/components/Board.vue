<template>
  <div>
    <!-- very bad solution -->
    <!-- 1. side list? what is "inline-flex"? -->
    <!-- 2. App.css .sideways = Spaghetto -->
    <!-- 3. use a table? -->
    <!--
    <div>
      <ol>
        <li v-for="i1 in width" :key="i1 - 1">
          <ol class="sideways">
            <li v-for="i2 in width" :key="i2 - 1">
              <Square
                :value="squares[(i1 - 1) * 3 + (i2 - 1)]"
                @click="$emit('click', (i1 - 1) * 3 + (i2 - 1))"
              />
            </li>
          </ol>
        </li>
      </ol>
    </div> -->

    <!-- tried a table:-->
    <tbody>
      <tr v-for="i1 in 3" :key="i1 - 1">
        <td v-for="i2 in 3" :key="i2 - 1">
          <Square
            :value="squares[(i1 - 1) * 3 + (i2 - 1)]"
            :marked="marked[(i1 - 1) * 3 + (i2 - 1)]"
            @click="$emit('click', (i1 - 1) * 3 + (i2 - 1))"
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
}

</script>