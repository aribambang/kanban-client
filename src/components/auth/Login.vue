<template>
  <div id="login">
    <div class="container">
      <div class="row w-100" style="height: 100vh">
        <div class="col d-flex justify-content-center">
          <div class="card w-50 my-auto">
            <div class="card-body">
              <h3 class="text-center">Login</h3>
              <form @submit.prevent="handleLogin">
                <div class="form-group">
                  <label class="text-muted" for="emailLogin"
                    >Email address</label
                  ><input
                    type="email"
                    class="form-control"
                    id="emailLogin"
                    v-model="email"
                    placeholder="Enter email"
                  />
                </div>
                <div class="form-group">
                  <label class="text-muted" for="passwordLogin">Password</label>
                  <div class="input-group" id="passwordInput">
                    <input
                      class="form-control"
                      id="passwordLogin"
                      :type="passwordType"
                      v-model="password"
                      placeholder="Enter password"
                    />
                    <div class="input-group-append">
                      <span class="input-group-text"
                        ><a href="#" @click="switchPassword"
                          ><i
                            :class="[
                              passwordType === 'password'
                                ? 'fa fa-eye-slash'
                                : 'fa fa-eye',
                            ]"
                            aria-hidden="true"
                          ></i></a
                      ></span>
                    </div>
                  </div>
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary btn-block">
                    Login
                  </button>
                </div>
              </form>
              <div class="d-flex justify-content-center">
                <p>Login with Social Media</p>
              </div>
              <div class="d-flex justify-content-center">
                <div id="google-signin-button"></div>
              </div>

              <hr />
              <div class="mt-2">
                Don't have an account?
                <a href="#" @click="toRegister"> Register here</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Login',
  data() {
    return {
      passwordType: 'password',
      email: '',
      password: '',
    };
  },
  props: ['host'],
  methods: {
    toRegister() {
      this.$emit('changeActivePage', 'Register');
    },
    switchPassword() {
      this.passwordType =
        this.passwordType === 'password' ? 'text' : 'password';
    },
    handleLogin() {
      axios({
        url: this.host + '/users/login',
        method: 'post',
        data: {
          email: this.email,
          password: this.password,
        },
      })
        .then((response) => {
          this.email = '';
          this.password = '';
          localStorage.setItem('access_token', response.data.access_token);
          localStorage.setItem(
            'full_name',
            `${response.data.first_name} ${response.data.last_name}`
          );
          this.$emit('changeActivePage', 'Home');
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
    onSignIn(googleUser) {
      const idToken = googleUser.getAuthResponse().id_token;
      axios({
        method: 'post',
        url: this.host + '/users/google',
        data: {
          idToken,
        },
      })
        .then((response) => {
          console.log(response);
          localStorage.setItem('access_token', response.data.access_token);
          localStorage.setItem(
            'full_name',
            `${response.data.first_name} ${response.data.last_name}`
          );
          this.$emit('changeActivePage', 'Home');
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
  },
  mounted() {
    gapi.signin2.render('google-signin-button', {
      onsuccess: this.onSignIn,
      longtitle: true,
    });
  },
};
</script>

<style></style>
