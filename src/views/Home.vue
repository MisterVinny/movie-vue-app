<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <input
        v-model="newMovie.title"
        type="text"
        placeholder="New Movie Title"
      />
      <input v-model="newMovie.year" type="text" placeholder="New Movie Year" />
      <input v-model="newMovie.plot" type="text" placeholder="New Movie Plot" />
      <input
        v-model="newMovie.director"
        type="text"
        placeholder="New Movie Director"
      />

      <button v-on:click="createNewMovie">Create New Movie</button>
    </div>

    <div v-for="movie in movies" v-bind:key="movie.id">
      <p><b>Title: </b>{{ movie.title }}</p>
      <p><b>Year: </b>{{ movie.year }}</p>
      <p><b>Plot: </b>{{ movie.plot }}</p>
      <button v-on:click="showMovie(movie)">More Info</button>
      <hr />
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Info</h1>
        <!-- <img src="" alt="" /> -->
        <p><b>Title: </b> {{ displayMovie.title }}</p>
        <p><b>Year: </b> {{ displayMovie.year }}</p>
        <p><b>Plot: </b> {{ displayMovie.plot }}</p>
        <p><b>Director: </b> {{ displayMovie.director }}</p>
        <p><b>Actors: </b> {{ displayMovie.actors }}</p>
        <p><b>Genres: </b> {{ displayMovie.genre_names }}</p>
        <button>Close</button>
      </form>
    </dialog>
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
      message: "The Movie Bin!",
      movies: [],
      newMovie: {},
      displayMovie: {},
    };
  },

  created: function () {
    this.indexMovies();
  },

  methods: {
    indexMovies: function () {
      axios.get("http://localhost:3000/movies").then((response) => {
        console.log(response.data);
        this.movies = response.data;
        this.movies.sort((a, b) => a.id - b.id).reverse();
      });
    },

    createNewMovie: function () {
      var params = this.newMovie;
      axios
        .post("http://localhost:3000/movies", params)
        .then((response) => {
          console.log(response.data);
          this.movies.push(response.data);
          this.movies.sort((a, b) => a.id - b.id).reverse();
        })
        .catch((error) => {
          console.log(error.response.data.errors);
        });
      this.newMovie = {};
    },

    showMovie: function (movie) {
      this.displayMovie = movie;
      console.log(this.displayMovie);
      document.querySelector("#movie-details").showModal();
    },
  },
};
</script>
