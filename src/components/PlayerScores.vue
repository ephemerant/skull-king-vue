<template>
  <div class="player-scores">
    <span
      class="player-score"
      :class="{ active: player.active }"
      v-for="(player, index) in players"
      :key="index"
    >
      <span class="player">{{ player.name }}:</span>
      <span class="score">{{ player.score }}</span>
    </span>
  </div>
</template>

<script>
export default {
  name: "PlayerScores",
  props: {
    players: Array,
  },
};
</script>

<style scoped>
.player-scores {
  display: flex;
  background: black;
  color: white;
  padding: 1rem;
  justify-content: space-around;
  font-size: 1.5rem;
}

.player-score {
  background: #222;
  border-radius: 1rem;
  padding: 0.5rem 1rem;
}

.player-score.active {
  background: #06a;
}

.player {
  font-weight: bold;
}

.score {
  margin-left: 0.5rem;
}
</style>
