<template>
  <div class="favorites-index">
    <h1>Favorites</h1>
    <p>partner: {{ currentUser.partner }}</p>
    <div v-for="favorite in favorites" v-bind:key="favorite.id">
      <h2>{{ favorite.movie.original_title }}</h2>
      <span class="image">
        <img
          v-bind:src="`https://image.tmdb.org/t/p/w300${favorite.movie.poster_path}`"
          v-bind:alt="favorite.movie.name"
        />
        <img :src="favorite.movie.poster_path" alt="" />
      </span>
      <p>{{ favorite.movie.overview }}</p>
      <br />
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      favorites: [],
      currentUser: {},
    };
  },
  created: function () {
    axios.get("/favorites").then((response) => {
      console.log("favorites index", response);
      this.favorites = response.data;
    });
    axios.get(`/users/${this.$parent.getUserId()}`).then((response) => {
      console.log(response.data);
      this.currentUser = response.data;
    });
  },
  methods: {},
};
</script>
