<template>
  <div class="movies-edit">
    <div class="d-flex justify-content-center">
      <div>
        <form v-on:submit.prevent="editPost()">
          <h1>Edit Movie</h1>
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
              autofocus
            />
          </div>
          <div class="form-group">
            <label>Year: </label>
            <input
              type="number"
              class="form-control"
              v-model="editMovieParams.year"
            />
          </div>
          <div class="form-group">
            <label>Plot: </label>
            <textarea
              id="newMovieForm"
              type="text"
              maxLength="500"
              class="form-control"
              v-model="editMovieParams.plot"
              cols="30"
              rows="10"
              placeholder="plot"
            ></textarea>
          </div>
          <small class="danger-text" v-if="editMovieParams.plot.length < 15"
            >Plot must be 15 to 500 characters in length.</small
          >
          <small v-if="editMovieParams.plot.length >= 15"
            >{{ 500 - editMovieParams.plot.length }} characters remaining of
            500.</small
          >
          <div class="form-group">
            <label>Director: </label>
            <input
              type="text"
              class="form-control"
              v-model="editMovieParams.director"
            />
          </div>
          <div class="form-group">
            <div class="form-check">
              <input
                v-model="editMovieParams.english"
                class="form-check-input"
                type="checkbox"
                value="true"
                id="flexCheckDefault"
              />
              <label class="form-check-label" for="flexCheckDefault">
                English
              </label>
            </div>
          </div>
        </form>
        <router-link :to="`/movies/${editMovieParams.id}`">
          Return to Movie View
        </router-link>
        | <button v-on:click="editMovie()">Submit Movie Update</button> |
        <button v-on:click="deleteMovie()">Destroy This Movie</button>
      </div>
    </div>
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
