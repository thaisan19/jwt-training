<template>
  <div class="login-main main">
    <div class="login-card card">
      <div class="login-icon icon">
        <img src="https://img.icons8.com/ios-filled/100/000000/login-as-user.png"/>
      </div>
      <div class="login-form form">
        <form name="form" @submit.prevent="handleLogin">
          <div class="form-group">
            <input 
              placeholder="Username..."
              type="text"
              v-model="user.username"
              required
              class="form-control"
              name="username">
          </div>
          <!-- <div class="error">
            Username is required!
          </div> -->
          <div class="form-group">
            <input 
              placeholder="Password..."
              type="password"
              v-model="user.password"
              required
              class="form-control"
              name="password">
          </div>
          <!-- <div class="error">
            Password is required!
          </div> -->
          <div class="form-group">
            <button class="login-button button" :disabled="loading">
              <span v-show="loading" class="loading-text"></span>
              <span>Login</span>
            </button>
          </div>
        </form>

        <div class="form-group">
          <div
          v-if="message"
          class="message">
            {{ message }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import User from '../models/user'

export default {
  name: 'Login',
  data() {
    return {
      user: new User('', ''),
      loading: false,
      message: ''
    }
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    }
  },
  created() {
    if (this.loggedIn) {
      this.$router.push('/profile');
    }
  },
  methods: {
    handleLogin() {
      this.loading = true;
      if (this.user.username && this.user.password) {
        this.$store.dispatch('auth/login', this.user).then(
          () => {
            this.$router.push('/profile');
          },
          error => {
            this.loading = false;
            this.message = (error.response && error.response.data) || error.message || error.toString();
          }
        )
      }
    }
  }
}
</script>

<style>
.main {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 80vh;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
.card {
  padding: 2em;
  background: #fdfdfd;
  border: 2px solid #2f2f2f;
  background: linear-gradient(270deg, #a7fae5, #a7c5fa, #b9a7fa);
  background-size: 600% 600%;
  animation: AnimationName 30s ease infinite;
  border-radius: 10px;
}
.form {
  margin-top: 1em;
}
.form-group:nth-child(2) {
  margin-top: 1em;
}
.form-group:nth-child(3) {
  margin-top: 2em;
}
.form-group > input {
  margin-left: .5em;
  padding: .5em 1em;
  border: 1px solid #2f2f2f;
  outline: none;
  border-radius: 5px;
  font-size: 1.2em;
}
.form-group button {
  padding: .5em 1em;
  font-size: 1.2em;
  text-transform: uppercase;
  background: #000;
  border: none;
  color: #fff;
  cursor: pointer;
  border-radius: 10px;
}

@keyframes AnimationName {
  0%{background-position:0% 50%}
  50%{background-position:100% 50%}
  100%{background-position:0% 50%}
}
</style>