<template>
  <div ref="containerRef" class="container" :style="{ gridTemplateColumns: `repeat(${columns}, 1fr)` }">
    <Item class="head-item"
      :class="{fullscreen: isFullscreen, docked: isDocked}"
      @click="isFullscreen = !isFullscreen"
      :data="items[0]"
    >
      {{items[0].name}}
    </Item>
    <div
      v-for="(item, index) in items"
      :key="index"
      :ref="setItemEle"
      @click="show = getinfo(itemEles[index])"
    >
      <Item :data="item" >
        {{ item.name }}
      </Item>
    </div>
  </div>
  <div class="controls">
    <label>每行元素数量:</label>
    <input type="range" v-model="columns" min="1" :max="maxItemCount" />
    <label>{{columns}} / {{maxItemCount}}</label>
  </div>
  <pre>{{ show }}</pre>
</template>

<script lang="ts" setup>
import {ref, computed, onMounted, toRaw, reactive} from 'vue';
import Item from './Item.vue';

let itemEles: HTMLElement[] = [];
function setItemEle(el: any){
  // get el html dom element
  itemEles.push(el);
}

function getinfo(el: HTMLElement) {
  // more position info
  let pa = el.parentNode as HTMLElement;
  return ' '+el.offsetWidth+' x '+el.offsetHeight
  +' \n'+el.offsetLeft+' x '+el.offsetTop
  +' \n'+el.getBoundingClientRect().left+' x '+el.getBoundingClientRect().top
  +' \n'+pa.offsetLeft+' x '+pa.offsetTop
  +' \n'+el.clientLeft+' x '+el.clientTop
  +' \n'+el.scrollLeft+' x '+el.scrollTop
}

let show = ref('');

let columns = ref(4);

let isFullscreen = ref(false)
let isDocked = ref(true);

interface Props {
  minItemWidth: number,
  items: any[]
}

const props = defineProps<Props>();

const items = reactive(props.items);

const containerRef = ref<InstanceType<typeof HTMLElement>>();

let maxItemCount = ref(10);

onMounted(()=>{
  if( containerRef.value !== undefined )
  maxItemCount.value = Math.floor(containerRef.value.offsetWidth / props.minItemWidth);
});

</script>
  
<style scoped>
.container {
  display: grid;
  /* grid-template-columns: repeat(4, 1fr); */
  grid-gap: 5px;
  background-color: #533;
  position: relative;
  height: 80vh;
  overflow-y: scroll;
}

.head-item{
  background:#c99;
    top: 100px;
    left: 100px;
    position: absolute; 
    width: 300px; 
    height: 300px; 
    transition: all 1s ease;
}

.head-item.docked{
    top: 0;
    left: 0;
    position: absolute; 
    width: 120px; 
    height: 100%; 
    transition: all 1s ease;
}

.head-item.fullscreen{
    z-index: 9999; 
    top: 0;
    left: 0;
    width: 100%; 
    height: 100%; 
    transition: all 1s ease;
}

</style>