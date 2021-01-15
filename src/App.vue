<template>
  <div>
    <navbar
      v-if="activePage !== 'Login' && activePage !== 'Register'"
      :activePage="activePage"
      @changeActivePage="changeActivePage"
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
      @fetchMembers="fetchMembers"
      @fetchBoards="fetchBoards"
      @addOrg="addOrg"
      @deleteOrg="deleteOrg"
      :orgsList="orgsList"
      :membersList="membersList"
      :boardsList="boardsList"
      @inviteMember="inviteMember"
      @deleteMember="deleteMember"
      @addBoard="addBoard"
      @openBoard="openBoard"
      ref="home"
    ></home>
    <board
      v-else-if="activePage === 'Board'"
      :board="board"
      :taskCategoriesList="taskCategoriesList"
      @addTask="addTask"
      @deleteTask="deleteTask"
      @updateTask="updateTask"
      @changeCatTask="changeCatTask"
      @addCategory="addCategory"
      @deleteCategory="deleteCategory"
      ref="board"
    ></board>
  </div>
</template>

<script>
import Navbar from './components/header/Navbar';
import Alert from './components/alert/Alert';
import Login from './components/auth/Login';
import Register from './components/auth/Register';
import Home from './components/home/Home';
import Board from './components/board/Board';
import axios from 'axios';
import io from 'socket.io-client';

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
      membersList: [],
      boardsList: [],
      boardCategories: [],
      taskCategoriesList: [],
      board: null,
      socket: io('localhost:3000'),
    };
  },
  components: {
    Alert,
    Login,
    Register,
    Home,
    Navbar,
    Board,
  },
  methods: {
    changeActivePage(page) {
      this.activePage = page;
    },
    openBoard(b) {
      this.board = b;
      this.fetchBoardCategories(b.id);
      this.fetchTask(b.id);
      this.changeActivePage('Board');
    },
    checkAuth() {
      if (localStorage.getItem('access_token')) {
        this.changeActivePage('Home');
        this.socket.on('TASKS', (data) => {
          if (data.boardId === this.board.id) {
            this.taskCategoriesList = data.taskCategoriesList;
          }
        });
      } else {
        this.changeActivePage('Login');
      }
    },
    showAlert({ t, m }) {
      this.activeAlert = true;
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
          this.showAlert({ t: 'success', m: 'Organization has been created' });
          this.$refs.home.hideOrgForm();
          this.fetchOrganizations();
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
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
            this.showAlert({ t: 'error', m: e.message });
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
          this.showAlert({ t: 'success', m: response.data.message });
          this.fetchOrganizations();
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    inviteMember(v) {
      axios({
        url: this.host + '/organizations/' + v.id + '/member',
        method: 'post',
        data: v.body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({
            t: 'success',
            m: 'Member has been invited to Organization',
          });
          this.$refs.home.hideInviteForm();
          this.fetchMembers(v.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    fetchMembers(id) {
      axios({
        url: this.host + '/organizations/' + id + '/member',
        method: 'get',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.membersList = response.data;
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    deleteMember(v) {
      axios({
        url: this.host + '/organizations/' + v.id + '/member/' + v.member,
        method: 'delete',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: response.data.message });
          this.fetchMembers(v.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },

    addBoard(body) {
      axios({
        url: this.host + '/boards',
        method: 'post',
        data: body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: 'Board has been created' });
          this.$refs.home.hideOrgForm();
          this.fetchBoards(body.OrganizationId);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },

    fetchBoards(id) {
      axios({
        url: this.host + '/boards/organization/' + id,
        method: 'get',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.boardsList = response.data;
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    fetchBoardCategories(id) {
      axios({
        url: this.host + '/boards/' + id,
        method: 'get',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.boardCategories = response.data;
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    addTask(body) {
      axios({
        url: this.host + '/tasks',
        method: 'post',
        data: body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: 'Task has been created' });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    deleteTask(id) {
      axios({
        url: this.host + '/tasks/' + id,
        method: 'delete',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: response.data.message });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    updateTask(v) {
      axios({
        url: this.host + '/tasks/' + v.idTask,
        method: 'put',
        data: v.body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: 'Task has been updated' });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    changeCatTask(v) {
      axios({
        url: this.host + '/tasks/' + v.id + '/category/' + v.idCat,
        method: 'patch',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: 'Task has been moved' });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    fetchTask(id) {
      axios({
        url: this.host + '/categories/board/' + id,
        method: 'get',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.taskCategoriesList = response.data;
          this.socket.emit('UPDATE_TASK', {
            taskCategoriesList: response.data,
            boardId: this.board.id,
          });
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    addCategory(body) {
      axios({
        url: this.host + '/categories',
        method: 'post',
        data: body,
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: 'Category has been created' });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
          });
        });
    },
    deleteCategory(id) {
      axios({
        url: this.host + '/categories/' + id,
        method: 'delete',
        headers: {
          authorization: 'Bearer ' + localStorage.getItem('access_token'),
        },
      })
        .then((response) => {
          this.showAlert({ t: 'success', m: response.data.message });
          this.fetchTask(this.board.id);
        })
        .catch((err) => {
          err.response.data.map((e) => {
            this.showAlert({ t: 'error', m: e.message });
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
