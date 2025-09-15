<script setup lang="ts">
import section1 from './components/section1.vue'
import section2 from './components/section2.vue'
import section3 from './components/section3.vue'
</script>

<template>
  <section1 />
  <section2 />
  <section3 />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
