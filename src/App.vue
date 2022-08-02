<template>
  <Game />
</template>

<script>
import Game from "@/components/Game.vue";

export default {
  name: "App",
  components: {
    Game,
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: Cambria, Cochin, Georgia, Times, "Times New Roman", serif;
}

body,
html,
#app {
  height: 100%;
}

* {
  user-select: none;
}
</style>
