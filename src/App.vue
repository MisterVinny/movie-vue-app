<template>
  <div id="app">
    <div id="nav">
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="32"
            height="32"
            fill="currentColor"
            class="bi bi-film"
            viewBox="0 0 16 16"
          >
            <path
              d="M0 1a1 1 0 0 1 1-1h14a1 1 0 0 1 1 1v14a1 1 0 0 1-1 1H1a1 1 0 0 1-1-1V1zm4 0v6h8V1H4zm8 8H4v6h8V9zM1 1v2h2V1H1zm2 3H1v2h2V4zM1 7v2h2V7H1zm2 3H1v2h2v-2zm-2 3v2h2v-2H1zM15 1h-2v2h2V1zm-2 3v2h2V4h-2zm2 3h-2v2h2V7zm-2 3v2h2v-2h-2zm2 3h-2v2h2v-2z"
            />
          </svg>
          <button
            class="navbar-toggler"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navbarNav"
            aria-controls="navbarNav"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span class="navbar-toggler-icon"></span>
          </button>
          <div
            class="collapse navbar-collapse justify-content-between"
            id="navbarNav"
          >
            <ul class="navbar-nav">
              <li class="nav-item">
                <a class="nav-link" href="/">Home</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="/movies">Movies</a>
              </li>
              <li v-if="isLoggedIn()" class="nav-item">
                <a class="nav-link" href="/movies/new">New Movie</a>
              </li>
            </ul>
            <ul class="navbar-nav" mr-auto>
              <li v-if="isLoggedIn()" class="nav-item">
                <router-link class="nav-link" to="/logout">Logout</router-link>
              </li>
              <li v-if="!isLoggedIn()" class="nav-item">
                <router-link class="nav-link" to="/signup">Signup</router-link>
              </li>
              <li v-if="!isLoggedIn()" class="nav-item">
                <router-link class="nav-link" to="/login">Login</router-link>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </div>
    <router-view />
  </div>
</template>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
  font-size: 1.5em;
} */
</style>

<script>
export default {
  data: function () {
    return {};
  },

  created: function () {},

  methods: {
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
  },
};
</script>
