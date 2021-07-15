<template>
  <div class="movies-show">
    <section id="home" class="hero-area style2">
      <div class="container">
        <div class="row align-items-center">
          <div class="col-lg-6 col-md-12 col-12">
            <div class="hero-content wow fadeInLeft" data-wow-delay=".3s">
              <h1>
                {{ movie.original_title }}
                <p>Release Date {{ movie.release_date }}</p>
              </h1>
              <p>
                {{ movie.overview }}
              </p>
              <br />
              <div class="button">
                <button v-on:click="createLike()" type="submit" class="btn">
                  Like
                </button>
                <button v-on:click="disLike()" type="submit" class="btn">
                  Dislike
                </button>
              </div>
              <!-- <iframe
                :src="`https://www.youtube.com/embed/${movie.videos.results[0].key}`"
                width="560"
                height="315"
                frameborder="0"
                allowfullscreen
              ></iframe> -->

              <a
                :href="`https://youtu.be/${movie.videos.results[0].key}`"
                target="_blank"
                class="glightbox video-button"
                ><i class="lni lni-play"></i
                ><span class="text">Click here for trailer</span></a
              >
            </div>
          </div>
          <div class="col-lg-6 col-md-12 col-12">
            <div class="hero-image wow fadeInRight" data-wow-delay=".4s">
              <span class="image">
                <img
                  v-bind:src="`https://image.tmdb.org/t/p/w300${movie.poster_path}`"
                  v-bind:alt="movie.name"
                />
                <img :src="movie.poster_path" alt="" />
              </span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- <h2>{{ movie.original_title }}</h2>

    <span class="image">
      <img
        v-bind:src="`https://image.tmdb.org/t/p/w300${movie.poster_path}`"
        v-bind:alt="movie.name"
      />
      <img :src="movie.poster_path" alt="" />
    </span>
    <br />
    <iframe
      width="420"
      height="290"
      :src="`https://www.youtube.com/embed/${movie.videos.results[0].key}`"
    ></iframe>
    <p>Release Date {{ movie.release_date }}</p>
    <p>Overview {{ movie.overview }}</p>
    <button v-on:click="disLike()">Dislike</button>
    <button v-on:click="createLike()">Like</button> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      movie: {},
      user: {},
    };
  },
  created: function () {
    axios.get("/movies/random").then((response) => {
      console.log("movies show", response);
      this.movie = response.data;
      console.log(this.movie["videos"]["results"][0]["key"]);
    });
    // axios.get(`/users/${this.$parent.getUserId()}`).then((response) => {
    //   console.log(response.data);
    //   this.user = response.data;
    //   console.log(this.user.likes);
    // });
  },
  methods: {
    createLike: function () {
      var params = { api_movie_id: this.movie.id };
      axios.post("/likes", params).then((response) => {
        console.log(response.data);
        // if favorite is returned instead of a like.
        if (response.data.relationship_id) {
          this.$parent.flashMessage = "Favorite created!";
          // Trying to make an audio alert for when a favorite is added.
          // var alert = new Audio(require(audio/Ding.mp3)).play(alert);
        }
      });
      axios.get("/movies/random").then((response) => {
        console.log("movies show", response);
        this.movie = response.data;
        // if (response.data === this.movie) {
        //   axios.get("/movies/random").then((response) => {
        //     console.log("movies show", response);
        //     this.movie = response.data;
        //   });
        // } else {
        //   this.movie = response.data;
        // }
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
