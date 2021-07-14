<template>
  <div class="favorites-index">
    <section id="features" class="features section">
      <div class="container">
        <div class="row">
          <div class="col-12">
            <div class="section-title">
              <h2 class="wow fadeInUp" data-wow-delay=".4s">
                {{ currentUser.name }} & {{ currentUser.partner.name }}'s
                Watchlist
              </h2>
              <p class="wow fadeInUp" data-wow-delay=".6s">
                Scroll down to see a full list of movies you and your partner
                have matched on.
              </p>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-lg-4 col-md-6 col-12">
            <!-- Start Single Feature -->
            <div class="single-feature wow fadeInUp" data-wow-delay=".2s">
              <i>
                <img :src="currentUser.image_url" alt="" />
              </i>
              <h3>{{ currentUser.username }}</h3>
              <p>
                Appmeet is a completely creative, lightweight, clean & super
                responsive app landing page.
              </p>
            </div>
            <!-- End Single Feature -->
          </div>
          <div class="col-lg-4 col-md-6 col-12">
            <!-- Start Single Feature -->
            <div class="single-feature wow fadeInUp" data-wow-delay=".4s">
              <i class="lni lni-heart"></i>
              <h3>Scroll down to view movies from your watchlist</h3>
              <i class="lni lni-heart"></i>
            </div>
            <!-- End Single Feature -->
          </div>
          <div class="col-lg-4 col-md-6 col-12">
            <!-- Start Single Feature -->
            <div class="single-feature wow fadeInUp" data-wow-delay=".6s">
              <i><img :src="currentUser.partner.image_url" alt="" /></i>
              <h3>{{ currentUser.partner.username }}</h3>
              <p>
                Appmeet is a completely creative, lightweight, clean & super
                responsive app landing page.
              </p>
            </div>
            <!-- End Single Feature -->
          </div>
        </div>
      </div>
    </section>
    <!-- Start Blog Singel Area -->
    <section class="section blog-section blog-list">
      <div class="container">
        <div class="row">
          <div class="col-md-12 col-12">
            <div class="row">
              <div
                v-for="favorite in favorites"
                v-bind:key="favorite.id"
                class="col-lg-4 col-md-4 col-12"
              >
                <!-- Start Single Blog -->
                <div class="single-blog">
                  <div class="blog-img">
                    <a href="blog-single-sidebar.html"
                      ><img
                        class="thumb"
                        v-bind:src="`https://image.tmdb.org/t/p/w300${favorite.movie.poster_path}`"
                        v-bind:alt="favorite.movie.name"
                    /></a>
                  </div>
                  <div class="blog-content">
                    <span class="date"
                      ><i class="lni lni-calendar"></i>
                      {{ favorite.movie.release_date }}</span
                    >
                    <h4 class="title">
                      {{ favorite.movie.original_title }}
                    </h4>
                    <p>
                      {{ favorite.movie.overview.slice(0, 100) + "..." }}
                    </p>
                    <div class="meta-details">
                      <a
                        :href="`https://youtu.be/${favorite.movie.videos}`"
                        target="_blank"
                        ><i class="lni lni-youtube"></i>
                        <span style="padding-left: 5px">See Trailer</span></a
                      >
                      <a v-on:click="deleteFavorite(favorite)" target="_blank"
                        ><i
                          style="padding-left: 10px"
                          class="lni lni-trash"
                        ></i>
                        <span style="padding-left: 5px"
                          >Remove from Watchlist</span
                        ></a
                      >
                    </div>
                  </div>
                </div>
                <!-- End Single Blog -->
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- End Blog Singel Area -->

    <!-- <h1>Our Watchlist</h1>
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

      <iframe
        width="420"
        height="315"
        :src="`https://www.youtube.com/embed/${favorite.movie.videos}`"
      ></iframe>

      <br />
      <button v-on:click="deleteFavorite(favorite)">
        Remove from Watchlist
      </button>
    </div> -->
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
    axios.get(`/users/${this.$parent.getUserName()}`).then((response) => {
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
