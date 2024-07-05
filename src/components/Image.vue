<template>
  <div class="img-box">
    <img :src="src" />
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';

interface Props {
  src: string;
}

let props = defineProps<Props>();

</script>

<style scoped>
.img-box {
  /* width: 100%; */
  /* height = width */
  aspect-ratio: 1;
  max-width: 100%;
  max-height: 100%;
  margin: 0.5em;
  overflow: hidden;
  border-radius: 0.5em;
  box-shadow: 0 0 1em #0003;
  background: #fff3;
  transition: transform 300ms;
}
img {
  max-width: 100%;
  min-width: 50px;
  height: auto;
}
</style>