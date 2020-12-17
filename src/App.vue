<template>
  <div class="container">
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

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

* {
  box-sizing: border-box;
}

html {
  font-size: 62.5%;
}

body {
  font-family: "Roboto", sans-serif;
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  overflow: hidden;
  background: #757f9a; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #d7dde8,
    #757f9a
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #d7dde8,
    #757f9a
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}

.player-card {
  width: 30rem;
  height: 80vh;
  border-radius: 3rem;
  background-color: #e3e3ed;
  margin: 5rem;
  padding: 2rem 2rem;
  box-shadow: 0 0 0.5rem 0.5rem rgba(0, 0, 0, 0.1);
}
</style>
