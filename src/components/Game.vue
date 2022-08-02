<template>
  <div class="game">
    <PlayerScores :players="players" />
    <GameArea
      :active-card="activeCard"
      :played-cards="playedCards"
      :card-dropped="cardDropped"
    />
    <PlayerCards :cards="cards" :lead-card="leadCard" />
  </div>
</template>

<script>
import PlayerScores from "@/components/PlayerScores.vue";
import GameArea from "@/components/GameArea.vue";
import PlayerCards from "@/components/PlayerCards.vue";

export default {
  name: "Game",
  components: { PlayerScores, GameArea, PlayerCards },
  data() {
    return {
      players: [
        { name: "Clayton", score: 50, active: false },
        { name: "Stephen", score: 60, active: true },
        { name: "Amanda", score: 20, active: false },
        { name: "Rebecca", score: 50, active: false },
      ],
      leadCard: null,
      playedCards: [],
      cards: [
        CreateCard("yellow", 14),
        CreateCard("green", 1),
        CreateCard("escape"),
        CreateCard("pirate"),
        CreateCard("tigress"),
        CreateCard("black", 14),
        CreateCard("skull-king"),
        CreateCard("purple", 11),
        CreateCard("purple", 7),
        CreateCard("black", 4)
      ],
    };
  },
  created() {},
  methods: {
    cardDropped(event) {
      const cardId = event.dataTransfer.getData("cardId");
      const cardIndex = this.cards.findIndex((card) => card.id == cardId);
      const card = this.cards.splice(cardIndex, 1)[0];

      this.playCard(card);
    },
    playCard(card) {
      if (!this.leadCard || this.leadCard.type === "escape")
        this.leadCard = card;

      // Commit Tigress pirate/escape logic fork
      if (card.subType) {
        card.type = card.subType;
        card.subTypeText =
          card.type.slice(0, 1).toUpperCase() + card.type.slice(1);
        card.color = GetColor(card.type);
      }

      this.playedCards.push(card);
    },
  },
  computed: {
    activeCard() {
      if (this.playedCards.length)
        return this.playedCards[this.playedCards.length - 1];

      return null;
    },
  },
};

let cardId = 0;

function CreateCard(type, number) {
  const id = ++cardId;
  const color = GetColor(type);

  switch (type) {
    case "skull-king":
      return { id, type, color, name: "Skull King" };
    case "pirate":
      return { id, type, color, name: "Pirate" };
    case "tigress":
      return { id, type, subType: "pirate", color, name: "Tigress" };
    case "escape":
      return { id, type, color, name: "Escape" };
    default:
      return {
        id,
        type,
        color,
        number,
        name: number,
        bonus: CalculateBonus(type, number),
      };
  }
}

function CalculateBonus(type, number) {
  if (type === "black" && number === 14) return 20;
  if (type === "yellow" && number === 14) return 10;
}

function GetColor(type) {
  switch (type) {
    case "pirate":
    case "tigress":
      return "#333";
    case "skull-king":
      return "#222";
    case "escape":
      return "#30a";
    case "yellow":
      return "#c90";
    case "green":
      return "green";
    case "purple":
      return "#70a";
    case "black":
      return "#444";
  }
}
</script>

<style scoped>
.game {
  display: flex;
  flex-flow: column;
  height: 100%;
}
</style>
