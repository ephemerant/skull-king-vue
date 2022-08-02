<template>
  <div class="game-area">
    <div class="played-cards">
      <Card
        v-for="(card, index) in playedCards.slice(0, -1)"
        :card="card"
        :key="index"
        :class="{ winning: card === winningCard }"
      />
    </div>

    <Card
      :card="activeCard"
      :class="{ winning: activeCard === winningCard }"
      @drop="cardDropped($event)"
      @dragover.prevent
      @dragenter.prevent
    />
  </div>
</template>

<script>
import Card from "@/components/Card.vue";

export default {
  name: "GameArea",
  components: { Card },
  props: {
    activeCard: Object,
    playedCards: Array,
    cardDropped: Function,
  },
  computed: {
    winningCard() {
      let leadingCard = null;
      let bestCard = null;

      this.playedCards.forEach((card) => {
        if (!leadingCard || leadingCard.type === "escape") leadingCard = card;

        if (!bestCard || IsWinningCard(card, bestCard)) bestCard = card;
      });

      return bestCard;
    },
  },
};

function IsWinningCard(card, bestCard) {
  if (card.type === "skull-king") return true;
  if (bestCard.type === "skull-king") return false;

  if (card.type === "escape") return false;
  if (bestCard.type === "escape") return true;

  if (bestCard.type === "pirate") return false;
  if (card.type === "pirate") return true;

  if (card.type === "black" && bestCard.type !== "black") return true;
  if (bestCard.type === "black" && card.type !== "black") return false;

  if (card.type === bestCard.type) return card.number > bestCard.number;

  return false;
}
</script>

<style scoped>
.game-area {
  height: 100%;
  background: radial-gradient(#060, #111);
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
}

.played-cards {
  display: flex;
  transform: scale(0.5, 0.5);
}
</style>
