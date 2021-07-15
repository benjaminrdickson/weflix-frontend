<template>
  <div id="app">
    <!-- Start Navbar -->
    <nav class="navbar navbar-expand-lg">
      <router-link class="navbar-brand style2" to="/movies">
        <img
          src="https://st.depositphotos.com/1555678/1276/i/200/depositphotos_12766135-stock-photo-3d-cinema-clapper-film-reel.jpg"
          alt="WeFlix"
          style="max-width: 30%"
        />
        WeFlix
      </router-link>
      <button
        class="navbar-toggler mobile-menu-btn"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="toggler-icon"></span>
        <span class="toggler-icon"></span>
        <span class="toggler-icon"></span>
      </button>
      <div
        class="collapse navbar-collapse sub-menu-bar"
        id="navbarSupportedContent"
      >
        <ul id="nav" class="navbar-nav ms-auto">
          <li v-if="isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              v-bind:to="`/users/${getUserName()}`"
              >Profile Page</router-link
            >
            >
          </li>
          <li v-if="isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              to="/movies"
              >Browse Movies</router-link
            >
            >
          </li>

          <li v-if="isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              to="/favorites"
              >Favorites</router-link
            >
            >
          </li>
          <li v-if="isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              to="/logout"
              >Logout</router-link
            >
            >
          </li>

          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              to="/login"
              >Login</router-link
            >
            >
          </li>
          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link
              class="page-scroll active"
              aria-label="Toggle navigation"
              to="/signup"
              >Sign Up</router-link
            >
            >
          </li>
        </ul>
      </div>
      <!-- navbar collapse -->
    </nav>
    <!-- End Navbar -->

    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <span v-if="isLoggedIn()">
        <router-link to="/movies">Movies</router-link>
        |
        <router-link v-bind:to="`/users/${getUserName()}`"
          >Profile Page</router-link
        >
        |
        <router-link to="/favorites">Favorites</router-link>
        |
        <router-link to="/logout">Logout</router-link>
      </span>
      <span v-else>
        <router-link to="/signup">Sign Up</router-link>
        |
        <router-link to="/login">Login</router-link>
      </span>
    </div> -->
    <div style="text-align: center" v-if="flashMessage">
      {{ flashMessage }}
      <div class="button">
        <button type="submit" class="btn" v-on:click="flashMessage = ''">
          Dismiss
        </button>
      </div>
    </div>
    <router-view :key="$route.fullPath" />
    <a href="#" class="scroll-top">
      <i class="lni lni-chevron-up"></i>
    </a>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      flashMessage: "",
    };
  },
  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
    getUserId: function () {
      return localStorage.getItem("user_id");
    },
    getUserName: function () {
      console.log(localStorage.getItem("username"));
      return localStorage.getItem("username");
    },
  },
};
</script>
