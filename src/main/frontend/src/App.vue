<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      Travel request platform
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Good morning {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Sign out</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button :class="isRegistering ? 'button-outline' : ''"
              @click="isRegistering = false">Sign in</button>
      <button :class="!isRegistering ? 'button-outline' : ''"
              @click="isRegistering = true">Sign up</button>
      <login-form @login="login($event)"
                  v-if="!isRegistering"></login-form>
      <login-form @login="register($event)"
                  :button-label="'Sign up'"
                  v-else="isRegistering"></login-form>

    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                authenticatedUsername: "",
                isRegistering: false
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },
            register(user) {
                this.$http.post('participants', user)
                    .then(response => {
                        alert("Success!");
                    })
                    .catch(response => {
                        alert("No success! Status: " + response.status)
                    });
            },
            logout() {
                this.authenticatedUsername = '';
            }
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }
</style>

