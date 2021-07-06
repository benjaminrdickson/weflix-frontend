<template>
  <div class="users-show">
    <h1>{{ user.name }}'s Profile Page</h1>
    <p>Name: {{ user.name }}</p>
    <p>username: {{ user.username }}</p>
    <p>Email: {{ user.email }}</p>
    <button v-on:click="displayEdit = !displayEdit">Edit Profile</button>
    <form v-if="displayEdit" v-on:submit.prevent="updateUser()">
      <ul>
        <li class="error" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <label>
        Name:
        <input type="text" v-model="editUserParams.name" />
      </label>
      <br />
      <label>
        Username:
        <input type="text" v-model="editUserParams.username" />
      </label>
      <br />
      <label>
        Email Address:
        <input type="text" v-model="editUserParams.email" />
      </label>
      <br />
      <label>
        Profile Picture:
        <input type="text" v-model="editUserParams.image_url" />
      </label>
      <br />
      <label>
        Password:
        <input type="text" v-model="editUserParams.password" />
      </label>
      <br />
      <label>
        Password Confirmation:
        <input type="text" v-model="editUserParams.password_confirmation" />
      </label>
      <br />
      <input type="submit" value="Save Changes" />
    </form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      user: {},
      errors: [],
      editUserParams: {},
      displayEdit: false,
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      console.log("User object", response.data);
      this.user = response.data;
    });
  },
  methods: {
    updateUser: function () {
      axios
        .patch(`/users/${this.$route.params.id}`, this.editUserParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/user/${response.data.id}`);
          this.user = response.data;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
        });
    },
  },
};
</script>
