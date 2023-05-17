<template>
  <div class="home">
    <HelloGame />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloGame from "@/components/HelloGame.vue"; // @ is an alias to /src

@Component({
  components: {
    HelloGame,
  },
})
export default class HomeView extends Vue {}
</script>
<style>
.home {
  width: 100vw;
  height: 100vh;
  background-color: #fdf0c4;
}
</style>
