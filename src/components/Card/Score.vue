<template>
  <div>
    {{ score }}
    <button :disabled='score===0' @click="$emit('update:score', 0)" >Reset</button>
  </div>
</template>

<script lang="ts">
import { Component,Prop,Vue } from "vue-property-decorator";

@Component
export default class Score extends Vue {
  @Prop()
  score!: number;
}
</script>
