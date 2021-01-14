<template>
  <div>
    <navbar
      v-if="activePage !== 'Login' && activePage !== 'Register'"
      @logout="logout"
    ></navbar>
    <alert
      :activeAlert="activeAlert"
      :type="typeAlert"
      :message="messageAlert"
      @hideAlert="hideAlert"
    ></alert
    ><login
      v-if="activePage === 'Login'"
      :host="host"
      @changeActivePage="changeActivePage"
      @showAlert="showAlert"
    ></login>
    <register
      v-else-if="activePage === 'Register'"
      :host="host"
      @changeActivePage="changeActivePage"
      @showAlert="showAlert"
    ></register>
    <home
      v-else-if="activePage === 'Home'"
      :host="host"
      @showAlert="showAlert"
      @fetchOrganizations="fetchOrganizations"
      @addOrg="addOrg"
      @deleteOrg="deleteOrg"
      :orgsList="orgsList"
      ref="home"
    ></home>
  </div>
</template>

<script>
import Navbar from './components/header/Navbar';
import Alert from './components/alert/Alert';
import Login from './components/auth/Login';
import Register from './components/auth/Register';
import Home from './components/home/Home';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      activePage: 'Login',
      host: 'http://localhost:3000',
      activeAlert: false,
      typeAlert: '',
      messageAlert: '',
      orgsList: [],
    };
  },
  components: {
    Alert,
    Login,
    Register,
    Home,
    Navbar,
  },
  methods: {
    changeActivePage(page) {
      this.activePage = page;
    },
    checkAuth() {
      if (localStorage.getItem('access_token')) {
        this.changeActivePage('Home');
      } else {
        this.changeActivePage('Login');
      }
    },
    showAlert({ t, m }) {
      this.activeAlert = true;
      console.log(t, m);
      this.typeAlert = t;
      this.messageAlert = m;
    },
    hideAlert() {
      this.activeAlert = false;
      this.typeAlert = '';
      this.messageAlert = '';
    },
    logout() {
      localStorage.clear();
      let auth2 = gapi.auth2.getAuthInstance();
      auth2.signOut().then(function () {});
      this.changeActivePage('Login');
    },
    addOrg(body) {
      axios({
        url: this.host + '/organizations',
        method: 'post',
        data: body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.$emit('showAlert', {
            t: 'success',
            m: 'Organization has been created',
          });
          this.$refs.home.hideOrgForm();
          this.fetchOrganizations();
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
    fetchOrganizations() {
      axios({
        url: this.host + '/organizations',
        method: 'get',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.orgsList = response.data;
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
    deleteOrg(id) {
      axios({
        url: this.host + '/organizations/' + id,
        method: 'delete',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.$emit('showAlert', {
            t: 'success',
            m: response.data.message,
          });
          this.fetchOrganizations();
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.$emit('showAlert', { t: 'error', m: e.message });
          });
        });
    },
  },
  mounted() {
    this.checkAuth();
  },
};
</script>

<style scoped></style>
