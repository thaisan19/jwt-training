<template>
  <div id="app">
    <nav class="navbar">
      <div class="navbar-nav">
        <li class="nav-item">
          <router-link to="/home" class="nav-link">
            Home
          </router-link>
        </li>
        <li v-if="showAdminBoard" class="nav-item">
          <router-link to="/admin" class="nav-link">
            Admin Board
          </router-link>
        </li>
        <li v-if="showModeratorBoard" class="nav-item">
          <router-link to="/mod" class="nav-link">
            Moderator Board
          </router-link>
        </li>
        <li class="nav-item">
          <router-link v-if="currentUser" to="/user" class="nav-link">
            User
          </router-link>
        </li>
      </div>

      <div v-if="!currentUser" class="navbar-nav">
        <li class="nav-item">
          <router-link to="/register" class="nav-link">
            Sign Up
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/login" class="nav-link">
            Login
          </router-link>
        </li>
      </div>

      <div v-if="currentUser" class="navbar-nav">
        <li class="nav-item">
          <router-link to="/profile" class="nav-link">
            {{ currentUser.username }}
          </router-link>
        </li>
        <li class="nav-item">
          <a href class="nav-link" @click.prevent="logOut">
            LogOut
          </a>
        </li>
      </div>
    </nav>

    <div class="container">
      <router-view/>
    </div>
  </div> 
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
.navbar,
.navbar-nav {
  display: flex;
  align-items: center;
  justify-content: center;
}
.navbar li {
  list-style: none;
  text-transform: uppercase;
  font-size: 1.4em;
  margin-top: 1em;
}
.navbar li:not(:first-child) {
  margin-left: 3em;
}
.navbar li a {
  text-decoration: none;
  color: #2f2f2f;
  transition: .5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
.navbar li a:hover {
  color:crimson;
  font-weight: 600;
}
/* .router-link-active {
  color:crimson;
  font-weight: 600;
} */
.router-link-exact-active {
  color:crimson;
  font-weight: 600;
}
</style>

<script>
export default {
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
    showAdminBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('ROLE_ADMIN');
      }

      return false;
    },
    showModeratorBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('ROLE_MODERATOR');
      }

      return false;
    }
  },
  methods: {
    logOut() {
      this.$store.dispatch('auth/logout');
      this.$router.push('/login');
    }
  }
};
</script>