<template>
  <div class="game-board">
    <div>score: {{ score }}</div>
    <div class="game-board-wrapper">
      <div
        v-for="(card, index) in cards"
        :key="index"
        class="card"
        @click="flipCard(index)"
      >
        <Card :card="card" :index="index" @flip="flipCard($event)" />
      </div>
    </div>
    <div class="btn-group">
      <button @click="backToNewGame">Back</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Card from "./Card";
import { ICard } from "../../interface/Card.s";

@Component({
  name: "game",
  components: {
    Card,
  },
  data() {
    return {
      cardCount: 0 as number,
      cards: [] as ICard[],
      flippedCards: [] as ICard,
      score: 0 as number,
    };
  },
  created() {
    this.cardCount = localStorage.getItem("level") | 0;
    this.randomDiziOlustur();
  },
  methods: {
    randomDiziOlustur(): ICard {
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
    },
    flipCard(index: number) : {
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
            this.score += 10;
            localStorage.setItem("score", this.score);
          } else {
            this.score--;
            localStorage.setItem("score", this.score);
            card1.isFlipped = false;
            card2.isFlipped = false;
          }
           this.flippedCards = [];
        }, 1000);
      }
    },
    backToNewGame() {
      this.$router.push("/");
    },
  },
})
export default class Cards extends Vue {}
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
