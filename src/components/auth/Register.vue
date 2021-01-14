<template>
  <div id="register">
    <div class="container">
      <div class="row w-100" style="height: 100vh">
        <div class="col d-flex justify-content-center">
          <div class="card w-50 my-auto">
            <div class="card-body">
              <h3 class="text-center">Register</h3>
              <form @submit.prevent="register">
                <div class="form-group">
                  <label class="text-muted" for="firstNameRegister"
                    >First Name</label
                  ><input
                    type="text"
                    class="form-control"
                    id="firstNameRegister"
                    v-model="first_name"
                    placeholder="Enter first name"
                  />
                </div>
                <div class="form-group">
                  <label class="text-muted" for="lastNameRegister"
                    >Last Name</label
                  ><input
                    type="text"
                    class="form-control"
                    id="lastNameRegister"
                    v-model="last_name"
                    placeholder="Enter last name"
                  />
                </div>
                <div class="form-group">
                  <label class="text-muted" for="emailRegister"
                    >Email address</label
                  ><input
                    type="email"
                    class="form-control"
                    id="emailRegister"
                    v-model="email"
                    placeholder="Enter email"
                  />
                </div>
                <div class="form-group">
                  <label class="text-muted" for="passwordRegister"
                    >Password</label
                  >
                  <div class="input-group" id="passwordInput">
                    <input
                      class="form-control"
                      :type="passwordType"
                      id="passwordRegister"
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
                    Register
                  </button>
                </div>
              </form>

              <hr />
              <div class="mt-2">
                Already have an account?
                <a href="#" @click="toLogin"> Login here</a>
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
  name: 'Register',
  data() {
    return {
      first_name: '',
      last_name: '',
      email: '',
      password: '',
      passwordType: 'password',
    };
  },
  props: ['host'],
  methods: {
    switchPassword() {
      this.passwordType =
        this.passwordType === 'password' ? 'text' : 'password';
    },
    register() {
      axios({
        url: this.host + '/users/register',
        method: 'post',
        data: {
          first_name: this.first_name,
          last_name: this.last_name,
          email: this.email,
          password: this.password,
        },
      })
        .then((response) => {
          this.first_name = '';
          this.last_name = '';
          this.email = '';
          this.password = '';
          this.$emit('showAlert', {
            t: 'success',
            m: 'Your account has been successfully created, you can login now',
          });
          this.$emit('changeActivePage', 'Login');
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
    toLogin() {
      this.$emit('changeActivePage', 'Login');
    },
  },
};
</script>

<style></style>
