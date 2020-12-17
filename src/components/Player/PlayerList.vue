<template>
  <div class="player-card player-list">
    <h3 class="now-playing">Now Playing</h3>
    <ul class="playlist">
      <li v-for="song in songs" :key="song.src" class="song-item">
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
</template>

<script>
import songs from "../../songs/songs.js";
export default {
  data() {
    return {
      songs: songs
    };
  }
};
</script>

<style>
.now-playing {
  padding: 1rem;
  background-color: #ffffff;
  width: 50%;
  margin: 0 auto;
  border-radius: 10px;
  box-shadow: 0 24px 35px -16px rgba(107, 179, 237, 0.5);
}

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

.actions > .delete {
  display: flex;
  align-items: center;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  justify-content: center;
  box-shadow: -1px 17px 24px -6px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  font-size: 20px;
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
  border: none;
  color: white;
}
</style>
