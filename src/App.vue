<template>
  <div class="">
    <player-card />
    <player-list />
  </div>
</template>

<script>
import PlayerCard from "./components/Player/PlayerCard.vue";
import PlayerList from "./components/Player/PlayerList.vue";
export default {
  components: { PlayerCard, PlayerList },
  name: "App"
};
</script>

<style></style>
