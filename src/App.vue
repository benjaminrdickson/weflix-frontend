<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <span v-if="isLoggedIn()">
        <router-link to="/movies">Movies</router-link>
        |
        <router-link v-bind:to="`/users/${getUserId()}`"
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
    </div>
    <div v-if="flashMessage">
      {{ flashMessage }}
      <button v-on:click="flashMessage = ''">Dismiss</button>
    </div>
    <router-view :key="$route.fullPath" />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

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
      console.log(localStorage.getItem("user_id"));
      return localStorage.getItem("user_id");
    },
    getUserName: function () {
      console.log(localStorage.getItem("username"));
      return localStorage.getItem("username");
    },
  },
};
</script>
