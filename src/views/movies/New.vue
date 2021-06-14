<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>Create New Movie</h1>

      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>

      <div class="form-group">
        <label>Title: </label>
        <input
          type="text"
          class="form-control"
          v-model="newMovieParams.title"
          placeholder="title"
        />
      </div>
      <div class="form-group">
        <label>Year: </label>
        <input
          type="text"
          class="form-control"
          v-model="newMovieParams.year"
          placeholder="year"
        />
      </div>
      <div class="form-group">
        <label>Plot: </label>
        <input
          type="text"
          class="form-control"
          v-model="newMovieParams.plot"
          placeholder="plot"
        />
      </div>
      <div class="form-group">
        <label>Director: </label>
        <input
          type="text"
          class="form-control"
          v-model="newMovieParams.director"
          placeholder="director"
        />
      </div>
      <!-- <div class="form-group">
        <label>English: </label>
        <input
          type="checkbox"
          class="form-control"
          v-model="newMovieParams.english"
          value="false"
        />
      </div> -->

      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
  </div>
</template>

<style>
p b {
  font-size: 1em;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newMovieParams: {},
      errors: [],
    };
  },

  created: function () {},

  methods: {
    createMovie: function () {
      axios
        .post("/movies", this.newMovieParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/movies");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
