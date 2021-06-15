<template>
  <div class="movies-index">
    <h1>The Movie Bin</h1>
    <p>
      Filter by:
      <input
        v-model="titleFilter"
        type="text"
        placeholder="title filter text"
      />
    </p>
    <span v-if="filterBy(movies, titleFilter, 'title').length == 0">
      No Results.
    </span>
    <hr />

    <div
      v-for="movie in filterBy(movies, titleFilter, 'title')"
      v-bind:key="movie.id"
    >
      <p><b>Title: </b>{{ movie.title }}</p>
      <p><b>Year: </b>{{ movie.year }}</p>
      <router-link :to="`/movies/${movie.id}`"> More Info </router-link>
      <hr />
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
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],

  data: function () {
    return {
      movies: [],
      titleFilter: "",
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
  },
};
</script>
