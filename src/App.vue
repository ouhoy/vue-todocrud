<template>

  <router-view/>
</template>


<script setup>
import "./index.css"
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 1200px;
  margin: 0 auto;

  color: #555;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}


.flex > [type=checkbox] {
  width: 24px;
  height: 24px;
  border-radius: 25px;


}

svg {
  width: 12px;
}
</style>
