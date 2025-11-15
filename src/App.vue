<template>
  <!-- This will display your menu component -->
  <MartMenu />
</template>

<script setup>
// 1. Import your component from the file
import MartMenu from './components/MartMenu.vue'
</script>

<style>
/* Reset default browser styles and set the app to be full-screen */
html, body, #app {
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
}

body {
  background-color: #f9f9f9;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
}

</style>