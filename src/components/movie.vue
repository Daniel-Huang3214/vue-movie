<script setup>
import axios from "axios"
import { ref } from "vue"

let popularity = ref(0);
let movieTrailer = ref("");
let poster = ref("");
let backdrop = ref("");
let title = ref("");
let originalTitle = ref("");
let release = ref("");
let overview = ref("");
let language = ref("");
let voteAverage = ref(0);
let voteCount = ref(0);

axios
  .get(`https://api.themoviedb.org/3/movie/424783`, {
    params: {
      api_key: "0dcabfe51b80fa2de3e80d7d256e0e81",
      append_to_response: "videos",
    },
  }).then((movieData) => {
    const trailers = movieData.data.videos.results.filter(
        (trailer) => trailer.type === "Trailer"
      );
    console.log(movieData.data);
    movieTrailer.value = `https://www.youtube.com/embed/${trailers.at(0).key}`;
    poster.value = `https://image.tmdb.org/t/p/w500${movieData.data.poster_path}`;
    backdrop.value = `https://image.tmdb.org/t/p/w500${movieData.data.backdrop_path}`;
    title.value = `${movieData.data.title}`;
    originalTitle.value = `${movieData.data.original_title}`;
    release.value = `Release Date: ${movieData.data.release_date}`;
    overview.value = `${movieData.data.overview}`;
    language.value = `Original Language: ${movieData.data.original_language}`;
    popularity.value = `Popularity: ${movieData.data.popularity}`;
    voteAverage.value = `${movieData.data.vote_average}` 
    voteCount.value = `${movieData.data.vote_count}`;
  });

</script>

<template>
  <select id="movies">
    <option value="424783">BumbleBee</option>
    <option value="68726">Pacific Rim</option>
    <option value="396535">Train to Busan</option>
    <option value="198663">Maze Runner</option>
    <option value="801604">Shark: The Beginning</option>
    <option value="346364">IT</option>
    <option value="1359">American Psycho</option>
    <option value="526896">Morbius</option>
    <option value="205321">Sharknado</option>
    <option value="447404">Detective Pikachu</option>
  </select>
  <button id="button" @click="onChange">Get</button>

  <div id="movie">
  <h1>{{ originalTitle }}</h1>
  <h2>{{ title }}</h2>
  <img id="poster" v-bind:src="`${poster}`">
  <p id="vote-average">| Vote Average: {{ voteAverage }}</p>
  <p id="vote-count">| Vote Count: {{ voteCount }}</p>
  <img id="background" v-bind:src="`${backdrop}`">
  <p id="release">{{ release }}</p>
  <p id="language">{{ language }}</p>
  <p id="popularity">| {{ popularity }}</p>
  <p id="overview">{{overview}}</p>
  <iframe v-bind:src="`${movieTrailer}`"></iframe>
  <div id="layer"></div>
</div>
</template>

<style scoped>
* {
  margin: 0px;
  border: 0px;
  padding: 0px;
}

h1 {
  color: white;
  font: bold;
  text-align: center;
  grid-column: 1;
  grid-row: 2;
}

h2 {
  color: white;
  font: bold;
  text-align: center;
  grid-column: 2;
  grid-row: 4;
}

iframe {
  width: 30vw;
  height: 40vh;
  grid-column: 3;
  grid-row: 10;
}

body {
  background-color: rgba(0, 0, 0, 0.795);
  overflow-x: hidden;
}

label {
  font: bold;
  color: white;
  font-size: 1.5rem;
}

#button {
  margin: 5px;
  width: 5vw;
  height: 3vh;
}

#release {
  color: white;
  font: bold;
  grid-column: 2;
  grid-row: 10;
}

#language {
  color: white;
  font: bold;
  grid-column: 2;
  grid-row: 11;
}

#popularity {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 3;
}

#movies {
  width: 15vw;
  height: 3vh;
}

#movie {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(20, 35px);
  justify-self: center;
  width: 500;
  justify-content: center;
}

#background {
  position: absolute;
  z-index: -2;
  width: 100%;
  height: 110%;
  grid-column: span 3;
  grid-row: span 10;
}

#layer {
  position: absolute;
  width: 100%;
  height: 110%;
  z-index: -1;
  background-color: black;
  opacity: 50%;
}

#poster {
  height: 90vh;
  width: 30vw;
  grid-column: 1;
  grid-row: 4;
}

#description {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 2;
}

#overview {
  color: white;
  font: bold;
  grid-column: 2;
  grid-row: 6;
}

#vote-average {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 4;
}

#vote-count {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 5;
}

</style>
