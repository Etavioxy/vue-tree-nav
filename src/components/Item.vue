<template>
  <div class="item">
    <Image :src="data.src"/>
    <slot></slot>
  </div>
</template>

<script setup>
import Image from './Image.vue';

const props = defineProps(['data']);

</script>

<style>
.item {
  flex: 1 1 0;
  box-sizing: border-box;
  padding: 10px;
  background-color: #101010;
  border: 1px solid #202020;
}
</style>