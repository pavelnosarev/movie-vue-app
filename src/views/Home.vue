<template>
  <div class="home">
    <div>
      Title:
      <input type="text" v-model="newMovieParams.title" />
      Director:
      <input type="text" v-model="newMovieParams.director" />
    </div>
    <button v-on:click="createMovie()">Add Movie</button>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h1>Title: {{ movie.title }}</h1>
      <p>Director: {{ movie.director }}</p>
      <!-- <img v-bind:src="movie.image_url" v-bind:alt="movie.name" /> -->
      <button v-on:click="showMovie(movie)">More Info</button>
      <button v-on:click="destroyMovie(movie)">Destroy Movie</button>
    </div>
    <dialog id="movie-details">
      <form method="dialog">
        <h1>Info for Movies</h1>
        <p>
          Title:
          <input type="text" v-model="currentMovie.title" />
        </p>
        <p>
          Director:
          <input type="text" v-model="currentMovie.director" />
        </p>
        <button v-on:click="updateMovie(currentMovie)">Update Movie</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Movies!",
      hipavel: "Hi Pavel.",
      movies: [],
      newMovieParams: {},
      currentMovie: {},
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios
        .get("http://localhost:3000/movies")
        .then((response) => {
          this.movies = response.data;
          console.log(this.movies);
        })
        .catch((error) => console.log(error.response));
    },
    createMovie: function () {
      axios
        .post("http://localhost:3000/movies", this.newMovieParams)
        .then((response) => {
          console.log("successfully added", response.data);
          this.movies.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showMovie: function (movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function (movie) {
      var editMovieParams = movie;
      axios
        .patch("http://localhost:3000/movies/" + movie.id, editMovieParams)
        .then((response) => {
          console.log("Success!", response.data);
        })
        .catch((error) => console.log(error.response));
    },
    destroyMovie: function (movie) {
      axios
        .delete("http://localhost:3000/movies/" + movie.id)
        .then((response) => {
          console.log("success", response.data);
          var index = this.movies.indexOf(movie);
          this.recipes.splice(index, 1);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
