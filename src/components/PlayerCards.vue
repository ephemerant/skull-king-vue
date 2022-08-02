<template>
  <div class="cards">
    <Card
      v-for="(card, index) in cards"
      :card="card"
      :key="index"
      :class="{
        unplayable: hasMatchingSuit && !playableCard(card, leadCard),
      }"
      draggable="true"
      @dragstart="dragCard($event, card)"
    />
  </div>
</template>

<script>
import Card from "@/components/Card.vue";

export default {
  name: "PlayerCards",
  components: { Card },
  props: {
    cards: Array,
    leadCard: Object,
  },
  methods: {
    dragCard(event, card) {
      event.dataTransfer.setData("cardId", card.id);
    },
    playableCard(card, leadCard) {
      if (
        leadCard &&
        leadCard.number &&
        card.number &&
        leadCard.type !== card.type
      )
        return false;

      return true;
    },
  },
  computed: {
    hasMatchingSuit() {
      return (
        this.leadCard &&
        this.leadCard.number &&
        this.cards.some(
          (card) => card.number && this.leadCard.type === card.type
        )
      );
    },
  },
};
</script>

<style scoped>
.cards {
  background: black;
  display: flex;
  align-items: center;
  overflow-x: auto;
  overflow-y: hidden;
  min-height: 15rem;
  max-height: 15rem;
}

.cards .card {
  cursor: grab;
}
</style>
