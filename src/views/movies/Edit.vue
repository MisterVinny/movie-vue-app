<template>
  <div class="movies-edit">
    <form v-on:submit.prevent="editPost()">
      <h1>Edit Move</h1>
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
          v-model="editMovieParams.title"
        />
      </div>
      <div class="form-group">
        <label>Year: </label>
        <input
          type="text"
          class="form-control"
          v-model="editMovieParams.year"
        />
      </div>
      <div class="form-group">
        <label>Plot: </label>
        <input
          type="text"
          class="form-control"
          v-model="editMovieParams.plot"
        />
      </div>
      <div class="form-group">
        <label>Director: </label>
        <input
          type="text"
          class="form-control"
          v-model="editMovieParams.director"
        />
      </div>
      <div class="form-group">
        <label>English: </label>
        <input
          type="checkbox"
          class="form-control"
          v-model="editMovieParams.english"
        />
      </div>
    </form>
    <router-link :to="`/movies/${editMovieParams.id}`">
      Return to Movie View
    </router-link>
    | <button v-on:click="editMovie()">Submit Movie Update</button> |
    <button v-on:click="deleteMovie()">Destroy This Movie</button>
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
      editMovieParams: {},
      errors: [],
    };
  },

  created: function () {
    this.showMovie();
  },

  methods: {
    showMovie: function () {
      axios.get(`/movies/${this.$route.params.id}`).then((response) => {
        console.log("Movie object: ", response.data);
        this.editMovieParams = response.data;
      });
    },

    editMovie: function () {
      axios
        .patch(`/movies/${this.$route.params.id}`, this.editMovieParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/movies/${response.data.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
        });
    },

    deleteMovie: function () {
      if (confirm("Do you actually want to delete this fine movie?")) {
        axios.delete(`/movies/${this.editMovieParams.id}`).then((response) => {
          console.log(response.data);
          this.$router.push(`/movies`);
        });
      }
    },
  },
};
</script>
