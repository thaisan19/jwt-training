<template>
  <div class="regsiter-main main">
    <div class="register-card card">
      <div class="register-icon icon">
        <img src="https://img.icons8.com/ios-filled/100/000000/login-as-user.png"/>
      </div>
      <div class="register-form form">
        <form name="form" @submit.prevent="handleRegister">
          <div v-if="!successful">
            <div class="form-group">
              <input type="text"
              placeholder="Username..."
              v-model="user.username"
              class="form-control"
              required
              name="username">
              <!-- <div v-if="submitted"
              class="error">
                Username...
              </div> -->
            </div>
            <div class="form-group">
              <input type="password"
              placeholder="Password..."
              v-model="user.password"
              class="form-control"
              required
              name="password">
              <!-- <div v-if="submitted"
              class="error">
                Password...
              </div> -->
            </div>
            <div class="form-group">
              <button class="login-button button">
                Sign up
              </button>
            </div>
          </div>
        </form>

        <div class="form-group">
          <div
          v-if="message"
          class="message"
          :class="successful ? 'alert-success' : 'alert-danger'">
            {{ message }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import User from '../models/user';

export default {
  name: 'Register',
  data() {
    return {
      user: new User('', '', ''),
      submitted: false,
      successful: false,
      message: ''
    }
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn
    }
  },
  mounted() {
    if (this.loggedIn) {
      this.$router.push('/profile')
    }
  },
  methods: {
    handleRegister() {
      this.message = ''
      this.submitted = true
      this.$store.dispatch('auth/register', this.user).then(
        data => {
          this.message = data.message
          this.successful = true
        },
        error => {
          this.message = (error.response && error.response.data) || error.message || error.toString()
          this.successful = false
        }
      )
    }
  }
}
</script>

<style>
.alert-success {
  margin-top: .5em;
  color: orange;
  font-weight: 600;
}
.alert-danger {
  margin-top: .5em;
  color: crimson;
  font-weight: 600;
}
</style>