<template>
  <div class="card" :class="{ empty: !card }">
    <div
      class="card-inner"
      v-if="card"
      :style="{ background: `linear-gradient(${card.color}, #111)` }"
    >
      <span>
        {{ card.name }}
      </span>

      <div class="card-bonus" v-if="card.bonus">Bonus {{ card.bonus }}</div>

      <div class="card-bonus" v-if="card.subTypeText">
        {{ card.subTypeText }}
      </div>

      <div class="tigress" v-if="card.type === 'tigress'">
        <button
          :class="{ selected: card.subType === 'pirate' }"
          @click="setCardSubType(card, 'pirate')"
        >
          Pirate
        </button>

        <button
          :class="{ selected: card.subType === 'escape' }"
          @click="setCardSubType(card, 'escape')"
        >
          Escape
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    card: Object,
  },
  methods: {
    setCardSubType(card, subType) {
      card.subType = subType;
    },
  },
};
</script>

<style scoped>
.card {
  min-width: 10rem;
  max-width: 10rem;
  min-height: 12rem;
  max-height: 12rem;
  background: white;
  margin: 1rem;
  padding: 0.25rem;
  border-radius: 1rem;
  color: white;
  text-shadow: 1px 2px 4px black;
  display: flex;
  justify-content: stretch;
  align-items: stretch;
}

.card.winning {
  background: yellow;
}

.card.unplayable {
  opacity: 0.3;
  pointer-events: none;
  filter: blur(2px);
}

.card.empty {
  background: rgba(0, 0, 0, 0.4);
}

.card-inner {
  background: #333;
  border-radius: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5rem;
  padding: 1rem;
  text-align: center;
  position: relative;
  width: 100%;
}

.card-bonus {
  position: absolute;
  right: 0.5rem;
  bottom: 0.5rem;
  font-size: 1.2rem;
}

.tigress {
  position: absolute;
  right: 0.5rem;
  bottom: 0.5rem;
  font-size: 1.2rem;
  display: flex;
  justify-content: space-between;
  left: 0.5rem;
}

.tigress button {
  color: white;
  background: #333;
  border: none;
  border-radius: 1rem;
  padding: 0.4rem 0.8rem;
  cursor: pointer;
  font-family: inherit;
  font-size: 1rem;
}

.tigress button.selected {
  background: #07a;
}
</style>
