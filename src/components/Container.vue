<template>
  <div ref="containerRef" class="container">
    <Item id="myDiv"
      :class="{fullscreen: isFullscreen}"
      @click="isFullscreen = !isFullscreen"
      :data="items[0]"
    >
      {{items[0].name}}
    </Item>
    <Item
      v-for="(item, index) in items"
      :key="index"
      :data="item"
    >
      {{ item.name }}
    </Item>
  </div>
</template>

<script setup>
import {ref, computed, onMounted} from 'vue';
import Item from './Item.vue';

let isFullscreen = ref(false)

const props = defineProps({
        minItemWidth: {
          type: Number,
          default: 200
        },
        minItemHeight: {
          type: Number,
          default: 200
        },
        items: {
          type: Array,
          required: true
        }
      })

const containerRef = ref(null);

onMounted(() => {
  console.log('Container width:', containerRef.value.offsetWidth)

  const itemsPerRow = computed(() => {
    return Math.floor(containerRef.value.offsetWidth / props.minItemWidth)
  })
  const itemWidth = computed(() => {
    return containerRef.value.offsetWidth / itemsPerRow.value
  })
  const itemHeight = computed(() => {
    return props.minItemHeight
  })
})
</script>
  
<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: flex-start;
}

#myDiv{
  background:#c99;
    top: 100px;
    left: 100px;
    position: fixed; 
    width: 300px; 
    height: 300px; 
    transition: all 1s ease;
}

#myDiv.fullscreen{
    z-index: 9999; 
    top: 0;
    left: 0;
    width: 100%; 
    height: 100%; 
    transition: all 1s ease;
}

</style>