<template>
  <div class="movies-show">
    <div>
      <h1>{{ movie.title }}</h1>
      <p><b>Year: </b>{{ movie.year }}</p>
      <p><b>Plot: </b>{{ movie.plot }}</p>
      <p><b>Director: </b>{{ movie.director }}</p>
      <p><b>English: </b>{{ movie.english }}</p>
      <router-link :to="`/movies/${movie.id}/edit`"> Edit Movie </router-link>
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
      movie: {},
    };
  },

  created: function () {
    this.showMovie();
  },

  methods: {
    showMovie: function () {
      axios.get(`/movies/${this.$route.params.id}`).then((response) => {
        console.log("Movie object: ", response.data);
        this.movie = response.data;
      });
    },
  },
};
</script>
