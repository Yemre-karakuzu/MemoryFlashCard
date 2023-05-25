<template>
  <div>
    {{ Score }}
    <button :disabled='value===0' @click="clearScore" >Reset</button>
  </div>
</template>

<script lang="ts">
import { Component,Prop,Vue } from "vue-property-decorator";

@Component
export default class Score extends Vue {
  @Prop()
  value!: number;
  
  clearScore(){
    this.value=0
    localStorage.setItem("score","0")
  }
  get Score(){
    return this.value
  }
}
</script>
