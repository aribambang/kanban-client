<template>
  <nav id="navbar" class="navbar navbar-expand-lg navbar-dark bg-info">
    <a class="navbar-brand" href="#"><b>Kanban</b></a>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a
            :class="['nav-link', activePage === 'Home' ? 'active' : '']"
            @click="changeActivePage('Home')"
            href="#"
            >Home</a
          >
        </li>
        <li class="nav-item">
          <a
            :class="['nav-link', activePage === 'Board' ? 'active' : '']"
            @click="changeActivePage('Board')"
            href="#"
            >Board</a
          >
        </li>
      </ul>
      <ul class="navbar-nav ml-auto">
        <li class="nav-item dropdown">
          <a
            class="nav-link active dropdown-toggle"
            href="#"
            id="navbarDropdown"
            role="button"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >
            <i class="fa fa-user"></i> {{ full_name }}
          </a>
          <div
            class="dropdown-menu dropdown-menu-right"
            aria-labelledby="navbarDropdown"
          >
            <a class="dropdown-item" @click="handleLogout" href="#"
              ><i class="fa fa-sign-out"></i> Logout</a
            >
          </div>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'Navbar',
  data() {
    return {
      full_name: localStorage.getItem('full_name'),
    };
  },
  props: ['activePage'],
  methods: {
    handleLogout() {
      this.$emit('logout');
    },
    changeActivePage(v) {
      this.$emit('changeActivePage', v);
    },
  },
};
</script>

<style></style>
