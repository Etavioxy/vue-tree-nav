<script setup lang="ts">
import Container from './components/Container.vue'
import png from './3m7.png';
import { Item } from './type.ts';

import { reactive } from 'vue';

const items = reactive([
  { name:'Item 1', src: png },
  {
    name:'Item 2', src: png,
    children: [
      { name:'Item 2-1', src: png },
      { name:'Item 2-2', src: png },
      { name:'Item 2-3', src: png },
      { name:'Item 2-4', src: png },
      { name:'Item 2-5', src: png },
      { name:'Item 2-6', src: png },
    ]
  },
  {
    name:'Item 3', src: png,
    children: [
      { name:'Item 3-1', src: png },
      { name:'Item 3-2', src: png },
      { name:'Item 3-3', src: png },
      { name:'Item 3-4', src: png },
    ]
  },
  { name:'Item 4', src: png },
  { name:'Item 5', src: png },
] as Item[]);

</script>

<template>
  <div style="padding: 1em;">
    <Container :min-item-width="120" :items="items" />
  </div>
</template>

<style>

.container {
  height: 75vh;
  width: 100%;
}

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
