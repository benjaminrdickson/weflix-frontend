<template>
  <div class="login">
    <section class="login section">
      <div class="container">
        <div class="row">
          <div class="col-lg-6 offset-lg-3 col-md-8 offset-md-2 col-12">
            <div class="form-head">
              <h4 class="title">Login</h4>
              <ul>
                <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
              </ul>
              <form v-on:submit.prevent="submit()" action="#!" method="post">
                <div class="form-group">
                  <label>Email</label>
                  <input type="email" v-model="email" />
                </div>
                <div class="form-group">
                  <label>Password</label>
                  <input type="password" v-model="password" />
                </div>
                <div class="button">
                  <button type="submit" class="btn">Login Now</button>
                </div>
                <p class="outer-link">
                  Don't have an account?
                  <a href="/signup">Signup here</a>
                </p>
              </form>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- <form v-on:submit.prevent="submit()">
      <h1>Login</h1>
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      <div>
        <label>Email:</label>
        <input type="email" v-model="email" />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="password" />
      </div>
      <input type="submit" value="Submit" />
    </form> -->
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        email: this.email,
        password: this.password,
      };
      axios
        .post("/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] =
            "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          localStorage.setItem("username", response.data.username);
          this.$parent.flashMessage = "Successfully logged in!";
          console.log(response.data.user_id);
          this.$router.push(`/users/${response.data.username}`);
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>
