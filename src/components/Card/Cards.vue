<template>
  <div class="game-board">
    <div>score: {{ score }}</div>
    <div class="game-board-wrapper">
      <div v-for="(card, index) in cards" :key="index" class="card" @click="flipCard(index)">
        <Card :cardItem="card" :index="index" @flip="flipCard($event)" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Card from "./Card.vue";
import { ICard } from "../../interface/Card";

@Component({
  name: "game",
  components: {
    Card,
  },
  // created() {
  //   this.cardCount = parseInt(localStorage.getItem("level"));
  //   this.randomDiziOlustur();
  // }  
})
export default class Cards extends Vue {
  cardCount = 0 as number;
  cards = [] as Array<ICard>;
  flippedCards = [] as Array<ICard>;
  score = "0" as string;
  created() {
    this.cardCount = parseInt(localStorage.getItem('level') || '');
    this.randomDiziOlustur();
  }
  randomDiziOlustur(): void {
    // 1'den cardCount/2'e kadar olan sayıları 2 kez diziye ekliyoruz
    for (let i = 1; i <= this.cardCount / 2; i++) {
      this.cards.push({ value: i, isMatch: false, isFlipped: false });
      this.cards.push({ value: i, isMatch: false, isFlipped: false });
    }
    // Diziyi karıştırıyoruz
    for (let i = this.cards.length - 1; i > 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      var temp = this.cards[i];
      this.cards[i] = this.cards[j];
      this.cards[j] = temp;
    }
  }
  flipCard(index: number) {
    const card: ICard = this.cards[index];

    // Zaten döndürülmüş kartı tekrar döndürmeyi engelle
    if (card.isFlipped || card.isMatch) {
      return;
    }

    card.isFlipped = true;
    this.flippedCards.push(card);

    if (this.flippedCards.length === 2) {
      // İki kart da döndürüldü, eşleşme kontrolü yap
      setTimeout(() => {
        const [card1, card2] = this.flippedCards;
        if (card1.value === card2.value) {
          card1.isMatch = true;
          card2.isMatch = true;
          this.score = (parseInt(this.score) + 10).toString();
          localStorage.setItem("score", this.score);
        } else {
          this.score = (parseInt(this.score) - 1).toString();
          localStorage.setItem("score", this.score);
          card1.isFlipped = false;
          card2.isFlipped = false;
        }
        this.flippedCards = [];
      }, 500);
    }
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
