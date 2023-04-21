<template>
  <div id="app">
    <header class="backdrop-blur sticky top-0 lg:text-left text-center">
      <nav>
          <router-link to="/">Countries around the World</router-link>
      </nav>
    </header>
    <router-view :key="$route.path"/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #0ea5e9;
}
</style>
