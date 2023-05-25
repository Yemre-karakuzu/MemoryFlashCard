<template>
  <div class="game-board">
    <Score v-model="score" />
    <div class="game-board-wrapper">
      <div v-for="(card, index) in cards" :key="index" class="card" @click="flipCard(index)">
        <Card :cardItem="card" :index="index" />
      </div>
    </div>
    <button @click="resetGame">Reset Game</button>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Card from "./Card.vue";
import Score from './Score.vue';
import { ICard } from "../../interface/Card";

@Component({
  name: "game",
  components: {
    Card,
    Score
  },
})
export default class Cards extends Vue {
  cardCount = 0 as number;
  cards = [] as Array<ICard>;
  flippedCards = [] as Array<ICard>;
  score = 0;
  created() {
    this.cardCount = parseInt(localStorage.getItem('level') || '');
    this.shuffleArray();
  }
  
  shuffleArray(): void {
    for (let i = 1; i <= this.cardCount / 2; i++) {
      this.cards.push({ value: i, isMatch: false, isFlipped: false });
      this.cards.push({ value: i, isMatch: false, isFlipped: false });
    }
    for (let i = this.cards.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      const temp = this.cards[i];
      this.cards[i] = this.cards[j];
      this.cards[j] = temp;
    }
  }
  flipCard(index: number) {
    const card: ICard = this.cards[index];
    if (card.isFlipped || card.isMatch) {
      return;
    }

    card.isFlipped = true;
    this.flippedCards.push(card);

    if (this.flippedCards.length === 2) {
      setTimeout(() => {
        const [card1, card2] = this.flippedCards;
        if (card1.value === card2.value) {
          card1.isMatch = true;
          card2.isMatch = true;
          this.score += 10;
        } else {
          this.score--;
          card1.isFlipped = false;
          card2.isFlipped = false;
        }
        localStorage.setItem("score", this.score.toString());
        this.flippedCards = [];
      }, 300);
    }
  }
  resetGame(){
    this.cards=[]
    this.shuffleArray();
  }
}
</script>
<style>
.game-board {
  display: flex;
  justify-content: space-around;
  align-items: center;
  height: 100%;
  flex-direction: column;
}

.game-board-wrapper {
  display: flex;
  flex-wrap: wrap;
  width: 320px;
}

.btn-group {
  display: flex;
  width: 20%;
  justify-content: space-between;
  align-items: center;
}

.card {
  width: 60px;
  height: 60px;
  background-color: lightblue;
  margin: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.card-content {
  font-size: 24px;
}
</style>
