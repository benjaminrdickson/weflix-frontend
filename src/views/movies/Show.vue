<template>
  <div class="movies-show">
    <h2>{{ movie.original_title }}</h2>
    <img v-bind:src="movie.poster_path" v-bind:alt="movie.name" />
    <p>Release Date {{ movie.release_date }}</p>
    <p>Overview {{ movie.overview }}</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
    };
  },
  created: function () {
    axios.get(`/movies/random${this.original_title}`).then((response) => {
      console.log("movies show", response);
      this.movie = response.data;
    });
  },
  methods: {},
};
</script>
