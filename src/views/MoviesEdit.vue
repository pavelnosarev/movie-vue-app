<template>
  <div class="movies-edit">
    <form v-on:submit.prevent="updateMovie()">
      <h1>Edit Movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="currentMovieParams.title" />
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="currentMovieParams.director" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      currentMovieParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/movies/${this.$route.params.id}`).then((response) => {
      console.log("Movie info:", response.data);
      this.currentMovieParams = response.data;
    });
  },
  methods: {
    updateMovie: function () {
      axios.patch(`/movies/${this.$route.params.id}`, this.currentMovieParams).then((response) => {
        console.log(response.data);
        this.$router.push(`/movies/${response.data.id}`);
      });
    },
    destroyMovie: function () {
      axios.delete(`/movies/${this.$route.params.id}`).then((response) => {
        console.log("Good job!", response.data);
        this.$router.push("/movies");
      });
    },
  },
};
</script>
