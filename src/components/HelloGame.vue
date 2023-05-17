<template>
  <div class="gameStart-container">
    <div class="gameStart-wrapper">
      <div>Flash Card Game</div>
      <div>
        <label for="level">Choose a card count:</label>
        <select v-model="level" id="level">
          <option value="12">12</option>
          <option value="16" selected>16</option>
          <option value="20">20</option>
          <option value="24">24</option>
        </select>
      </div>
      <button @click="startGame">Start</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class HelloGame extends Vue {
  level = "16";
  startGame() {
    localStorage.setItem("level", this.level);
    this.$router.push("/game");
    localStorage.setItem("score", "0");
  }
}
</script>
<style>
.gameStart-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.gameStart-wrapper {
  display: flex;
  height: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}
</style>
