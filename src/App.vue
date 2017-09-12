<template>
  <div id="app">
    <header>
      <a href="/">PWArteg</a>
      <a href="https://github.com">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="28" height="28" viewBox="0 0 24 24"
          fill="none" stroke="currentColor"
          stroke-width="2" stroke-linecap="round"
          stroke-linejoin="round" class="feather feather-github">
          <path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path>
        </svg>
      </a>
      <a href="https://vuejs.org"><img src="./assets/logo.png" alt="Vue PWA"></a>
    </header>
    <main>
      <search></search>
    </main>
  </div>
</template>

<script>
import Search from './components/Search'

export default {
  name: 'app',
  components: {
    Search
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
</style>
