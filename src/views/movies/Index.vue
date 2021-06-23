<template>
  <div class="movies-index">
    <h1>The Movie Bin</h1>
    <p>
      Filter by:
      <input
        v-model="titleFilter"
        type="text"
        placeholder="title filter text"
        autofocus
      />
    </p>
    <span v-if="filterBy(movies, titleFilter, 'title').length == 0">
      No Results.
    </span>
    <hr />

    <div class="row row-cols-1 row-cols-md-4 g-4">
      <div
        v-for="movie in filterBy(movies, titleFilter, 'title')"
        v-bind:key="movie.id"
        class="col"
      >
        <div class="card h-100">
          <!-- <img src="..." class="card-img-top" alt="..." /> -->
          <div class="card-body">
            <h5 class="card-title">{{ movie.title }}</h5>
            <p class="card-text">{{ returnShort(movie.plot) }}</p>
            <small class="text-muted">{{ movie.year }}</small>
            <div class="card-footer">
              <router-link :to="`/movies/${movie.id}`"> More Info </router-link>
            </div>
          </div>
        </div>
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

    returnShort: function (plot) {
      return plot.substring(0, 25);
    },
  },
};
</script>
