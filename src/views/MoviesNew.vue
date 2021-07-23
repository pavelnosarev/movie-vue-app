<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>New Movie</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Title:</label>
        <input type="text" v-model="newMovieParams.title" />
      </div>
      <div>
        <label>Director:</label>
        <input type="text" v-model="newMovieParams.director" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      errors: [],
      newMovieParams: {},
    };
  },
  methods: {
    createRecipe: function () {
      console.log("Creating that movie!");
      axios.post("/movies", this.newMovieParams).then((response) => {
        this.$router.push("/movies");
        console.log(response.data);
      });
    },
  },
};
</script>
