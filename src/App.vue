<template>
    <div class="container">
        <div class="player-card player-music">
            <h3 class="heading-tertiary">Now playing</h3>

            <section class="player">
                <div class="cover-wrapper">
                    <img v-bind:class="coverObject" :src="current.cover" />
                </div>

                <div class="song-details">
                    <h2 class="song-title">
                        {{ current.title }}
                    </h2>
                    <p class="song-artist">{{ current.artist }}</p>
                </div>

                <KProgress
                    :show-text="false"
                    class="progress-bar-wrapper"
                    v-bind:percent="current.percent"
                    :color="['#F02E13', '#F46E19', '#F5AC18']"
                />

                <div class="timer">
                    <p class="start">{{ currentlyTimer }}</p>
                    <p class="end">
                        {{ current.totalTimer }}
                    </p>
                </div>

                <div class="controls">
                    <button class="prev" @click="prev">
                        <i class="fas fa-step-backward"></i>
                    </button>
                    <button class="play" v-if="!isPlaying" @click="play">
                        <i class="fas fa-play"></i>
                    </button>
                    <button class="pause" v-else @click="pause">
                        <i class="fas fa-pause"></i>
                    </button>
                    <button class="next" @click="next">
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
                    @click="play(song)"
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
                            :color="['#F02E13', '#F46E19', '#F5AC18']"
                            :show-text="false"
                            class="progress-bar-wrapper"
                            v-bind:percent="song.percent"
                        /> -->
                    </div>

                    <div class="actions">
                        <button class="delete">
                            <i class="fas fa-play"></i>
                        </button>
                    </div>
                </li>
            </ul>
        </div>

        <div class="footer">
            <p>
                I do not own the design or the music. <br />
                For the design, check here :
                <a href="https://dribbble.com/shots/14762728-Music-Player-App"
                    >https://dribbble.com/shots/14762728-Music-Player-App</a
                >
            </p>
        </div>
    </div>
</template>

<script>
import KProgress from 'k-progress-v3'
import songs from './songs/songs'
import { formatTimer } from './utils/utils'

export default {
    name: 'App',
    components: {
        KProgress
    },
    data() {
        return {
            current: {},
            coverObject: { cover: true, animated: false },
            index: 0,
            isPlaying: false,
            currentlyTimer: '00:00',
            songs: songs,
            player: new Audio()
        }
    },
    methods: {
        listenersWhenPlay() {
            this.player.addEventListener('timeupdate', () => {
                var playerTimer = this.player.currentTime
                this.currentlyTimer = formatTimer(playerTimer)
                this.current.percent =
                    (playerTimer * 100) / this.current.seconds
                this.current.isPlaying = true
            })
            this.player.addEventListener(
                'ended',
                function() {
                    this.next()
                }.bind(this)
            )
        },
        setCover() {
            this.coverObject.animated = true
            setTimeout(() => {
                this.coverObject.animated = false
            }, 1000)
        },
        setCurrentSong() {
            this.current = this.songs[this.index]
            this.play(this.current)
            this.setCover()
        },
        play(song) {
            if (typeof song.src !== 'undefined') {
                this.current.isPlaying = false
                this.index = this.songs.indexOf(this.current)
                this.current = song
                this.player.src = this.current.src
            }
            this.player.play()
            this.isPlaying = true

            this.listenersWhenPlay()
        },
        pause() {
            this.player.pause()
            this.isPlaying = false
        },
        next() {
            this.current.isPlaying = false
            this.index = this.songs.indexOf(this.current)
            this.index++
            if (this.index > this.songs.length - 1) {
                this.index = 0
            }
            this.setCurrentSong()
        },
        prev() {
            this.current.isPlaying = false
            this.index = this.songs.indexOf(this.current)
            this.index--
            if (this.index < 0) {
                this.index = this.songs.length - 1
            }
            this.setCurrentSong()
        }
    },
    mounted() {
        this.current = this.songs[this.index]
        this.player.src = this.current.src
    }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

* {
    box-sizing: border-box;
}

html {
    font-size: 62.5%;
}

body {
    font-family: 'Roboto', sans-serif;
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
    transition: 1s ease;
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
    transition: 1s ease;
}

button:hover .fas {
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

.fa-play,
.fa-pause {
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
    margin: 4rem auto 2rem;
}

.cover-wrapper img {
    width: 20rem;
    height: 20rem;
    object-fit: cover;
    border-radius: 50%;
    box-shadow: -1px 17px 24px -6px rgba(0, 0, 0, 0.2);
}

.player-music .song-details {
    text-align: center;
}

.player-music .song-title {
    font-size: 2.5rem;
    margin-bottom: 1rem;
}

.player-music .song-artist {
    font-size: 1.2rem;
    margin-bottom: 3rem;
}

.controls {
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
}

.play .fas.fa-play,
.fa-pause {
    height: 5rem;
    width: 5rem;
    font-size: 2.5rem;
}

.k-progress {
    width: 100%;
}

.k-progress-outer {
    padding-right: 0;
}

.footer {
    position: absolute;
    bottom: 5px;
    left: 2rem;
    color: #8c8e99;
}
</style>
