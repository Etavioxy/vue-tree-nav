<template>
  <div class="img-box">
    <img :src="src" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

let props = defineProps(['src']);

const imageRef = ref(null);

onMounted(() => {
  //console.log('Image width:', imageRef.value.width)
})
</script>

<style scoped>
.img-box {
  /* width: 100%; */
  /* height = width */
  aspect-ratio: 1;
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