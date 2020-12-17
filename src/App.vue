<template>
  <div class="container">
    <div class="player-card player-music">
      <h3 class="heading-tertiary">Now playing</h3>

      <section class="player">
        <div class="cover-wrapper">
          <img :src="current.cover" />
        </div>

        <div class="song-details">
          <h2 class="song-title">
            {{ current.title }}
          </h2>
          <p class="artist">{{ current.artist }}</p>
        </div>
        <!-- <KProgress
          :show-text="false"
          class="progress-bar-wrapper"
          v-bind:percent="current.percent"
          :color="['#df83f1', '#82279f', '#53cfe0']"
        /> -->

        <!-- <div class="timer">
            <p class="start">{{ currentlyTimer }}</p>
            <p class="end">
              {{ current.totalTimer }}
            </p>
          </div>
        </div> -->

        <div class="controls">
          <button class="prev">
            <i class="fas fa-step-backward"></i>
          </button>
          <button class="play">
            <i class="fas fa-play-circle"></i>
          </button>
          <button class="pause" @click="pause">
            <i class="fas fa-pause-circle"></i>
          </button>
          <button class="next">
            <i class="fas fa-step-forward"></i>
          </button>
        </div>

        <div class="footer">
          <p>Powered by Soundcloud</p>
        </div>
      </section>
    </div>

    <div class="player-card player-list">
      <h3 class="heading-tertiary">Playlist</h3>
      <ul class="playlist">
        <li
          v-for="song in songs"
          :key="song.src"
          class="song-item"
          @click="playSong(song)"
        >
          <div class="cover-playlist">
            <img class="cover" :src="song.cover" />
          </div>
          <div class="song-details">
            <h2 class="song-title">
              {{ song.title }}
            </h2>
            <p class="song-artist">{{ song.artist }}</p>
            <!-- <KProgress
            v-if="song.isPlaying"
            :color="['#df83f1', '#82279f', '#53cfe0']"
            :show-text="false"
            class="progress-bar-wrapper"
            v-bind:percent="song.percent"
          /> -->
          </div>

          <div class="actions">
            <button class="delete">
              <i class="fas fa-play-circle"></i>
            </button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import songs from "./songs/songs";
export default {
  name: "App",
  data() {
    return {
      current: {},
      coverObject: { cover: true, animated: false },
      index: 0,
      isPlaying: false,
      currentlyTimer: "00:00",
      songs: songs,
      player: new Audio()
    };
  },
  methods: {
    playSong(song) {
      if (typeof song.src !== "undefined") {
        this.current.isPlaying = false;
        this.index = this.songs.indexOf(this.current);
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    }
  },
  mounted() {
    this.current = this.songs[0];
    this.player.src = this.current.src;
  }
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

button {
  border: none;
  background-color: transparent;
}

button:focus {
  outline: none;
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

.heading-tertiary {
  padding: 1rem;
  background-color: #ffffff;
  width: 50%;
  margin: 0 auto;
  border-radius: 10px;
  box-shadow: 0 24px 35px -16px rgba(107, 179, 237, 0.5);
  letter-spacing: 1px;
  font-size: 1.4rem;
  text-align: center;
}

/* PLAYLIST STYLE */

.player-list {
  margin: 10rem 2rem 2rem !important;
  overflow-y: scroll;
  text-align: center;
}

.playlist {
  padding: 0;
  list-style-type: none;
  height: 90%;
  overflow-y: scroll;
}

.song-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  border-radius: 1rem;
  padding: 1rem;
  transition: 0.3s ease-in-out;
  cursor: pointer;
}

.song-item:hover {
  background-color: rgb(235, 235, 235);
}

.song-details {
  margin-right: auto;
  margin-left: 1rem;
  text-align: left;
}

.song-artist {
  color: #8c8e99;
  letter-spacing: 0.4px;
}

.cover-playlist > .cover {
  /* margin-right: 15px; */
  width: 50px;
  height: 50px;
}

.cover {
  height: 100%;
  width: 270px;
  box-shadow: 0 24px 35px -16px rgba(107, 179, 237, 0.5);
  border-radius: 3px;
  object-fit: cover;
}

.fas {
  display: flex;
  align-items: center;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  justify-content: center;
  box-shadow: -1px 17px 24px -6px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  font-size: 20px;
  border: none;
  color: #e1e6f2;
  background-color: #ffffff;
}

.fas.fa-play-circle {
  color: white;
  background: #f12711; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to top,
    #f5af19,
    #f12711
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to top,
    #f5af19,
    #f12711
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}

/*************/
/*************/
/* PLAYER CARD */
/*************/
/*************/

.player-music {
  margin: 2rem 2rem 10rem !important;
  position: relative;
}

.cover-wrapper {
  text-align: center;
  margin: 4rem auto;
}

.cover-wrapper img {
  width: 20rem;
  height: 20rem;
  object-fit: cover;
  border-radius: 50%;
  box-shadow: -1px 17px 24px -6px rgba(0, 0, 0, 0.2);
}

.controls {
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}

.play .fas.fas.fa-play-circle {
  height: 6rem;
  width: 6rem;
  font-size: 4rem;
}

.footer {
  position: absolute;
  bottom: 5px;
  left: 2rem;
  color: #8c8e99;
}
</style>
