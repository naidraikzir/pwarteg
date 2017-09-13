<template>
  <div id="app">
    <header>
      <a href="/pwarteg">PWArteg</a>
      <a href="https://github.com/naidraikzir/pwarteg">
        <github-icon></github-icon>
      </a>
      <a href="https://vuejs.org">
        <img src="./assets/logo.png" alt="Vue PWA">
      </a>
    </header>
    <main>
      <g-search></g-search>
    </main>
  </div>
</template>

<script>
import GithubIcon from './components/GithubIcon'
import GSearch from './components/GSearch'

export default {
  name: 'app',
  components: {
    GithubIcon,
    GSearch
  }
}
</script>

<style>
body {
  margin: 0;
  font: 16px/1.5 -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  color: #444;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

main {
  margin: 0 auto;
  max-width: 768px;
  padding: 1em;
}

header {
  align-items: center;
  display: flex;
  justify-content: space-between;
  margin: 0;
  height: 56px;
  padding: 0.25em 1em;
  background-color: #50f5b0;
}

header a {
  color: inherit;
  font-size: 20px;
  font-weight: 500;
  line-height: 1;
}

header a:first-child {
  margin-right: auto;
  text-decoration: none;
}

header img {
  margin-left: 1em;
  max-height: 40px;
}

section:not(:last-child) {
  border-bottom: 1px dashed #444;
}
</style>
