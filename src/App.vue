<script setup lang="ts">
import Container from './components/Container.vue'
import png from './3m7.png';

const items = [
  { name:'Item 1', src: png },
  { name:'Item 2', src: png },
  { name:'Item 3', src: png },
  { name:'Item 4', src: png },
  { name:'Item 5', src: png },
];

</script>

<template>
  <Container :items="items" />
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
