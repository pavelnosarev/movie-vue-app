<template>
  <div class="movies-index">
    <div v-for="movie in movies" :key="movie.id">
      <router-link v-bind:to="`/movies/${movie.id}`">
        <h2>Title: {{ movie.title }}</h2>
        <p>Director: {{ movie.director }}</p>
        <img v-bind:src="movie.image_url" alt="movie.title" />
      </router-link>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Here are all the movies!",
      movies: [],
    };
  },
  created: function () {
    this.indexMovies();
  },
  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        this.recipes = response.data;
        console.log("All recipes:", this.movies);
      });
    },
  },
};
</script>
