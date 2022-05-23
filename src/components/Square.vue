<template>
  <button
    :class="`square ${marked}`" 
    @click="$emit('click')">
    {{value}}
  </button>

</template>


<script lang="ts">
import Vue from "vue";
import Component from "vue-class-component";

import { Prop } from "vue-property-decorator";

@Component({})
export default class Square extends Vue {
  @Prop({}) value: "X" | "O" | null = null; //3.
  @Prop({}) marked: boolean = false; //4.

}
</script>