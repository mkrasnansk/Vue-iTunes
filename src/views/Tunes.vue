/* eslint-disable prettier/prettier */
<template>
  <div class="tunes">
    <h1>iTunes</h1>

<TunesSearchForm/>
<TunesList>
  <TunesListSong/>
  <TunesListSong/>
  <TunesListSong/>
  <TunesListSong/>
</TunesList>

    <form action="#" @submit.prevent="getMusic()">
      <input type="text" v-model="query" />
    </form>
    <ul>
      <li v-for="song in songs" :key="song.id">
        <div>
          <img v-if="song.cover" :src="song.cover" alt="altText" />
        </div>
        <p>
          <strong>
            {{ song | songify | upper }}
          </strong>
        </p>
        <figure v-if="song.audioFile">
          <figcaption>{{ song.collectionName }}</figcaption>
          <audio controls :src="song.audioFile"></audio>
        </figure>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      query: "",
      limit: 10,
      songs: [
        {
          id: 1,
          artist: "Gulian Gondola",
          name: "SebeC"
        },
        {
          id: 2,
          artist: "Anton fIlo",
          name: "Brat"
        },
        {
          id: 3,
          artist: "marian JUnak",
          name: "Otelo"
        },
        {
          id: 4,
          artist: "zaneta TraBant",
          name: "Nemozny"
        }
      ]
    };
  },
  methods: {
    getMusic() {
      console.log(this.query);

      axios
        .get(
          `https://itunes.apple.com/search?term=${encodeURI(
            this.query
          )}&entity=musicTrack
          &limit=${this.limit}`
        )
        .then(response => {
          console.clear();
          this.songs = [];
          response.data.results.forEach(song => {
            if (song.kind === "song") this.songs.push(this.extractData(song));
            console.log(this.extractData(song));
          });
        });
    },
    extractData({
      trackId: id,
      artistName: artist,
      previewUrl: audioFile,
      artworkUrl100: cover,
      trackName: name,
      collectionName: album
    }) {
      return { id, artist, audioFile, cover, name, album };
    }
  },
  filters: {
    songify: function(song) {
      return song.artist + " - " + song.name + " ";
    },
    upper(str) {
      return str
        .split(" ")
        .map(function(word, index) {
          if (index == 0) {
            return (
              word.charAt(0).toUpperCase() + word.slice(1).toLowerCase() + " "
            );
          }
          return (
            word.charAt(0).toUpperCase() + word.slice(1).toLowerCase() + " "
          );
        })
        .join("");
    }
  }
};
</script>

<style lang="scss" scoped>
ul {
  padding: 0;
  li {
    list-style: none;
  }
}
input {
  text-align:center;
  height: 2em;
  border: none;
  border-bottom: 1px solid #333;
  &:focus {
    border: none;
    outline: none;
  border-bottom: 1px solid #333;

  }
}
</style>
