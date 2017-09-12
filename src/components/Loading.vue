<template>
  <div class="loading-wrapper">
    <div class="loading"></div>
  </div>
</template>

<style>
.loading {
  animation: rotate 1s infinite;
  border: solid #444;
  border-width: 2px 2px 2px 0;
  border-radius: 50%;
  width: 3em;
  height: 3em;
  margin: auto;
}

@keyframes rotate {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>

