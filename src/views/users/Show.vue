<template>
  <div class="users-show">
    <!-- Service Details Start -->
    <div class="service-details section">
      <div class="container">
        <div class="content">
          <div class="row">
            <div class="col-lg-8 col-md-12 col-12">
              <div class="thumb">
                <img :src="user.image_url" :alt="user.username" />
              </div>
              <h3 class="title">
                {{ user.name }} ({{ user.username }})
                <a v-on:click="displayEdit = !displayEdit"
                  ><i class="lni lni-pencil-alt"></i
                ></a>
              </h3>

              <section
                v-if="displayEdit"
                class="login registration section"
                style="padding: 0px"
              >
                <div class="container">
                  <div class="row">
                    <div class="col-12">
                      <div class="form-head">
                        <h3 class="title">Edit Profile</h3>
                        <form v-on:submit.prevent="updateUser()">
                          <ul>
                            <li
                              class="error"
                              v-for="error in errors"
                              v-bind:key="error"
                            >
                              {{ error }}
                            </li>
                          </ul>
                          <div class="form-group">
                            <label>Name</label>
                            <input v-model="editUserParams.name" type="text" />
                          </div>
                          <div class="form-group">
                            <label>Username</label>
                            <input
                              v-model="editUserParams.username"
                              type="text"
                            />
                          </div>
                          <div class="form-group">
                            <label>Image Url</label>
                            <input
                              v-model="editUserParams.image_url"
                              type="text"
                            />
                          </div>
                          <div class="form-group">
                            <label>Email</label>
                            <input
                              v-model="editUserParams.email"
                              type="email"
                            />
                          </div>
                          <div class="form-group">
                            <label>Password</label>
                            <input
                              v-model="editUserParams.password"
                              type="password"
                            />
                          </div>
                          <div class="form-group">
                            <label>Confirm Password</label>
                            <input
                              v-model="editUserParams.password_confirmation"
                              type="password"
                            />
                          </div>

                          <div class="button">
                            <button type="submit" class="btn">
                              Save Changes
                            </button>
                          </div>
                        </form>
                      </div>
                    </div>
                  </div>
                </div>
              </section>
            </div>
            <div class="col-lg-4 col-md-12 col-12">
              <div class="service-sidebar">
                <div class="single-widget search-widget">
                  <h3>Find your partner</h3>
                  <form v-on:submit.prevent="findUser()">
                    <input
                      v-model="searchTerm"
                      type="text"
                      placeholder="Search Username..."
                    />
                    <button type="submit">
                      <i class="lni lni-search-alt"></i>
                    </button>
                  </form>
                </div>
                <div class="single-widget service-category">
                  <h3>Relationship Management</h3>
                  <ul>
                    <li v-if="user.id != $parent.getUserId() && !user.partner">
                      <a v-on:click="requestRelationship()">
                        Request Relationship <i class="lni lni-circle-plus"></i>
                      </a>
                    </li>
                    <li
                      v-if="
                        user.relationship &&
                        !user.relationship.confirmation &&
                        $parent.getUserId() == user.id
                      "
                    >
                      <a v-on:click="updateRelationship()">
                        Update Relationship <i class="lni lni-reload"></i>
                      </a>
                    </li>
                    <li
                      v-if="
                        user.relationship &&
                        user.partner &&
                        user.id == $parent.getUserId()
                      "
                    >
                      <a v-on:click="destroyRelationship()">
                        End Relationship <i class="lni lni-trash"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      user: {},
      errors: [],
      editUserParams: {},
      displayEdit: false,
      users: [],
      searchTerm: "",
    };
  },
  created: function () {
    axios.get(`/users/${this.$route.params.username}`).then((response) => {
      console.log(response.data);
      this.user = response.data;
      this.editUserParams = response.data;
    });
  },
  methods: {
    updateUser: function () {
      axios
        .patch(`/users/${this.user.id}`, this.editUserParams)
        .then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "User Updated!";
          this.displayEdit = false;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(error.response.data.errors);
        });
    },
    destroyUser: function () {
      if (
        confirm(
          "Are you sure you want to delete your profile? All data will be lost."
        )
      ) {
        axios
          .delete(`/users/${this.$route.params.username}`)
          .then((response) => {
            console.log(response.data);
            this.$parent.flashMessage = "User Deleted!";
            this.$router.push("/signup");
          })
          .catch((error) => {
            this.errors = error.response.data.errors;
          });
      }
    },
    requestRelationship: function () {
      var params = { recipient_id: this.user.id };
      axios.post("/relationships", params).then((response) => {
        console.log(response.data);
        this.$parent.flashMessage = "Request Sent!";
      });
    },
    updateRelationship: function () {
      axios
        .patch(`/relationships/${this.user.relationship.id}`)
        .then((response) => {
          console.log(response.data);
          this.user.relationship.confirmation = true;
          this.$parent.flashMessage = "Relationship Approved!";
        });
    },
    destroyRelationship: function () {
      axios
        .delete(`/relationships/${this.user.relationship.id}`)
        .then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "Relationship Ended!";
          this.$router.push(`/users/${this.$parent.getUserName()}`);
        });
    },
    findUser: function () {
      axios
        .get(`/users/${this.searchTerm}`)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/users/${response.data.username}`);
        })
        .catch(() => {
          this.errors = ["User does not exist"];
        });
    },
  },
};
</script>
