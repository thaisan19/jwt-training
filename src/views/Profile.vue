<template>
  <div class="profile-main main">
    <div class="profile-card card">
      <div class="profile icon icon">
        <img src="https://img.icons8.com/ios-filled/100/000000/login-as-user.png"/>
      </div>
      <div class="profile-detail form">
        <div class="profile-header">
          <h3>
            <strong>
              {{ currentUser.username }} Profile
            </strong>
          </h3>
        </div>
        <div class="profile-content">
          <p>
            <strong>
              Token:
            </strong> {{ currentUser.accessToken.subString(0, 20) }} ... {{currentUser.accessToken.substr(currentUser.accessToken.length - 20)}}
          </p>
          <p>
            <strong>
              Id:
            </strong> {{ currentUser.id }}
          </p>
          <p>
            <strong>
              Email:
            </strong> {{ currentUser.email }}
          </p>
          <strong>Authorities:</strong>
          <ul>
            <li v-for="(role,index) in currentUser.roles" :key="index">{{role}}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Profile',
  computed: {
    currentUser() {
      return this.$store.state.auth.user
    }
  },
  mounted() {
    if (!this.currentUser) {
      this.$router.push('/login')
    }
  }
}
</script>

<style>

</style>