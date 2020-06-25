<template>
  <div id="app">
    <b-navbar type="dark" variant="dark">
      <b-navbar-nav>
        <b-nav-item>
          <router-link :to="{ name: 'main' }"></router-link>Vue Recipes
        </b-nav-item>
        <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
        {{ !$root.store.username }}
        <b-nav-item v-if="!$root.store.username">
          <router-link :to="{ name: 'register' }">Register</router-link>|
          <router-link :to="{ name: 'login' }">Login</router-link>|
        </b-nav-item>
        <b-nav-item v-else>
          {{ $root.store.username }}:
          <button @click="Logout">Logout</button>|
        </b-nav-item>
        <!-- Navbar dropdowns -->
        <b-nav-item-dropdown text="Lang" right>
          <b-dropdown-item href="#">EN</b-dropdown-item>
          <b-dropdown-item href="#">ES</b-dropdown-item>
          <b-dropdown-item href="#">RU</b-dropdown-item>
          <b-dropdown-item href="#">FA</b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown text="User" right>
          <b-dropdown-item href="#">Account</b-dropdown-item>
          <b-dropdown-item href="#">Settings</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-navbar>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    Logout() {
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");

      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
