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
    <home v-else-if="activePage === 'Home'"></home>
  </div>
</template>

<script>
import Navbar from './components/header/Navbar';
import Alert from './components/alert/Alert';
import Login from './components/auth/Login';
import Register from './components/auth/Register';
import Home from './components/home/Home';

export default {
  name: 'App',
  data() {
    return {
      activePage: 'Login',
      host: 'http://localhost:3000',
      activeAlert: false,
      typeAlert: '',
      messageAlert: '',
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
  },
  mounted() {
    this.checkAuth();
  },
};
</script>

<style scoped></style>
