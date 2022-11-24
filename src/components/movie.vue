<script setup>
import axios from "axios"
import { ref } from "vue"

let movie = ref(false);
let movieId = ref(424783);
let movieTrailer = ref("");

const onChange = () => {
   axios
    .get(`https://api.themoviedb.org/3/movie/${movieId.value}`, {
      params: {
        api_key: "0dcabfe51b80fa2de3e80d7d256e0e81",
        append_to_response: "videos",
      },
    }).then((movieData) => {
      const trailers = movieData.data.videos.results.filter(
        (trailer) => trailer.type === "Trailer"
      );
      console.log(movieData.data);
      movie.value = movieData.data
      movieTrailer.value = `https://www.youtube.com/embed/${trailers.at(0).key}`;
    })
};

</script>

<template>
  <select v-model="movieId" id="movies">
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

  <div id="movie" v-if="movie">
    <h1>{{ movie.original_title }}</h1>
    <h2>{{ movie.title }}</h2>
    <img id="poster" v-bind:src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`">
    <p id="vote-average">| Vote Average: {{ movie.vote_average }}</p>
    <p id="vote-count">| Vote Count: {{ movie.vote_count }}</p>
    <img id="background" v-bind:src="`https://image.tmdb.org/t/p/w500${movie.backdrop_path}`">
    <p id="release">Release Date: {{ movie.release_date }}</p>
    <p id="language">Original Language: {{ movie.original_language }}</p>
    <p id="popularity">| Popularity: {{ movie.popularity }}</p>
    <p id="budget">| Budget: ${{ movie.budget }}</p>
    <p id="revenue">| Revenue: ${{ movie.revenue }}</p>
    <p id="overview">{{ movie.overview }}</p>
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
  grid-row: 6;
}

iframe {
  position: relative;
  width: 34vw;
  height: 40vh;
  grid-column: 3;
  grid-row: 15;
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
  grid-row: 15;
}

#language {
  color: white;
  font: bold;
  grid-column: 2;
  grid-row: 16;
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
  filter: blur(3px);
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
  height: 70vh;
  width: 30vw;
  grid-column: 1;
  grid-row: 5;
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
  grid-row: 10;
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

#budget {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 6;
}

#revenue {
  color: white;
  font: bold;
  grid-column: 3;
  grid-row: 7;
}
</style>
