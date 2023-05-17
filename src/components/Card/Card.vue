<template>
  <div @click="flipCardEmit" class="card-content">
    <div v-if="cardItem.isFlipped">{{ cardItem.value }}</div>
    <div v-else>?</div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { ICard } from "../../interface/Card";

@Component
export default class Card extends Vue {
  @Prop()
cardItem!: Array<ICard>;
  @Prop()
index!: number;
  flipCardEmit() {
    this.$emit("flip", this.index);
  }
}
</script>
