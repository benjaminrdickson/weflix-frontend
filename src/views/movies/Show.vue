<template>
  <div class="movies-show">
    <h2>{{ movie.original_title }}</h2>
    <span class="image">
      <img
        v-bind:src="`https://image.tmdb.org/t/p/w300${movie.poster_path}`"
        v-bind:alt="movie.name"
      />
      <img :src="movie.poster_path" alt="" />
    </span>
    <p>Release Date {{ movie.release_date }}</p>
    <p>Overview {{ movie.overview }}</p>
    <button v-on:click="disLike()">Dislike</button>
    <button v-on:click="createLike()">Like</button>
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
    axios.get("/movies/random").then((response) => {
      console.log("movies show", response);
      this.movie = response.data;
    });
  },
  methods: {
    createLike: function () {
      var params = { api_movie_id: this.movie.id };
      axios.post("/likes", params).then((response) => {
        console.log(response.data);
        // currently not regenerating a new movie after like
        this.$router.push("/movies/random");
      });
    },
    disLike: function () {
      axios.get("/movies/random").then((response) => {
        console.log("movies show", response);
        this.movie = response.data;
      });
    },
  },
};
</script>
