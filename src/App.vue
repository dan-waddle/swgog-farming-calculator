<template>
  <div id="app">
    <div id="nav">
      <div id="logo">SWGoH</div>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
      <router-link to="/calculator">Calculator</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
@font-face {
  font-family: "StarJedi";
  src: local("StarJedi"),
    url("./fonts/StarJedi.eot?#iefix") format("embedded-opentype"),
    url("./fonts/StarJedi.woff2") format("woff2"),
    url("./fonts/StarJedi.woff") format("woff"),
    url("./fonts/StarJedi-DGRW.ttf") format("truetype"),
    url("./fonts/StarJedi.svg#StarJedi") format("svg");
}

#app {
  font-family: StarJedi, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #e5e7e6;
  background: #1f1b24;
}

#logo {
  color: #b80c09;
  font-weight: regular;
  text-decoration: none;
  font-size: 50px;
}

#nav {
  padding: 30px;
}

#nav a {
  color: #b80c09;
  font-weight: regular;
  text-decoration: none;
}

#nav a.router-link-exact-active {
  text-decoration: underline;
}
</style>
