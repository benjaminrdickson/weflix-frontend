<template>
  <div class="favorites-index">
    <h1>Our Watchlist</h1>
    <p>{{ currentUser.name }}</p>
    <img :src="currentUser.image_url" alt="" />
    <p>partner: {{ currentUser.partner.name }}</p>
    <img :src="currentUser.partner.image_url" alt="" />
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
      <button v-on:click="deleteFavorite(favorite)">
        Remove from Watchlist
      </button>
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
  methods: {
    deleteFavorite: function (favorite) {
      if (
        confirm(
          "Are you sure you want to remove this movie from your watchlist?"
        )
      ) {
        axios.delete(`/favorites/${favorite.id}`).then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "Movie Deleted from Watchlist!";
          for (var i = 0; i < this.favorites.length; i++) {
            if (this.favorites[i] === favorite) {
              this.favorites.splice(i, 1);
            }
          }
        });
      }
    },
  },
};
</script>
