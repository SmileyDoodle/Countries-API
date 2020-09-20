<template>
  <div id="app">
    <Header></Header>
    <router-view/>
    <Footer></Footer>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'


export default {
  name: 'App',
  components: {
    Header,
    Footer
  }
}
</script>

<style>
html {
  background-color: var(--bg-color);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--font-color);
  display: grid;
  height: 100vh;
  grid-template-rows: auto 1fr auto;
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


/* Overwrite for dark-theme */
strong {
  color: var(--font-color);
}
p {
  color: var(--font-color);
}
button {
  color: var(--font-color);
}
span {
  color: var(--font-color);
}
::placeholder {
    color: var(--placeholder-color) !important;
}
:root {
    --font-color: #4a4a4a;
    --bg-color: #fff;
    --secondary-color: #fff;
    --heading-color: #363636;
    --border-color: #dbdbdb;
    --placeholder-color: #dbdbdb;
    --input-text: #363636;
}
[data-theme="dark"] {
    --font-color: #fff;
    --bg-color: #324455;
    --secondary-color: #435769;
    --heading-color: #fff;
    --border-color: #324455;
    --placeholder-color: #C8C8C8;
    --input-text: #fff;
}
</style>
