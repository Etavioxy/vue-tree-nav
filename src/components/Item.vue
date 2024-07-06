<template>
  <div class="item" :style="{backgroundColor: color}">
    <Image :src="data.src"/>
    <slot></slot>
  </div>
</template>

<script lang="ts" setup>
import Image from './Image.vue';

interface Props {
  data: any;
  color?: string;
}

const props = defineProps<Props>();

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