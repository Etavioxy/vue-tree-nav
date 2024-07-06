<template>
  <div ref="containerRef" class="container" :style="{ gridTemplateColumns: `repeat(${columns}, 1fr)` }">
    <TransitionGroup name="head-item-trans"
      @before-enter="onBeforeEnter"
      @enter="onEnter"
      @leave="onLeave"
      @after-leave="onAfterLeave"
    >
      <div
        v-for="(item, index) in headItems"
        :key="index"
        class="head-item"
        @click="popHeadItem(index)"
      >
        <ItemComponent :data="item" color="red" >
          {{ item.name }}
        </ItemComponent>
      </div>
    </TransitionGroup>
    <div
      v-for="(item, index) in items"
      :ref="setItemEle"
      @click="enterItem(itemEles[index], item)"
    >
      <ItemComponent :data="item" >
        {{ item.name }}
      </ItemComponent>
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
import {ref, computed, onMounted, toRaw, reactive, onBeforeUpdate, onUpdated, watch} from 'vue';
import ItemComponent from './Item.vue';

// https://www.vueframework.com/docs/v3/cn/guide/migration/array-refs.html
let itemEles: HTMLElement[] = [];

function setItemEle(el: any){
  itemEles.push(el);
}
onBeforeUpdate(()=>{
  itemEles = [];
});

let headItemOriginalStyle = ref<any>({ color: 'black' });

let show = ref('');

function getinfo(el: HTMLElement) {
  // more position info
  let pa = el.parentNode as HTMLElement;
  return '长宽'+el.offsetWidth+' x '+el.offsetHeight
  +' \n'+'在container的位置'+el.offsetLeft+' x '+el.offsetTop
  +' \n'+'在视窗的位置'+el.getBoundingClientRect().left+' x '+el.getBoundingClientRect().top
  +' \n'+'container的位置'+pa.offsetLeft+' x '+pa.offsetTop
  //+' \n'+'在视窗的位置'+el.clientLeft+' x '+el.clientTop
  //+' \n'+'在视窗的位置'+el.scrollLeft+' x '+el.scrollTop
}

import type { Item } from '../type.ts';

interface HeadItem extends Item {
  pos: {
    x: number,
    y: number,
    h: number,
    w: number,
  }
}

const headItems = reactive<HeadItem[]>([]);

let nowTransitionItem: HeadItem | null = null;
let nowTransitionElement: Element = document.createElement('div');

function enterItem(el: HTMLElement, item: Item) {
  show.value = getinfo(el);
  const newHeadItem = {
    ...item,
    pos: {
      x: el.offsetTop,
      y: el.offsetLeft,
      h: el.offsetHeight,
      w: el.offsetWidth,
    },
//let isFullscreen = ref(false)
//let isDocked = ref(true);
  };
  nowTransitionItem = newHeadItem;
  headItems.push(newHeadItem);
  //headItemOriginalStyle.value = {
  //  top: newHeadItem.pos.x + 'px',
  //  left: newHeadItem.pos.y + 'px',
  //  height: newHeadItem.pos.h + 'px',
  //  width: newHeadItem.pos.w + 'px',
  //};
}
function popHeadItem(index: number){
  if( index == headItems.length - 1 ){
    nowTransitionItem = headItems[index];
    headItems.pop();
  }
}

function getStyle(headItem: HeadItem){
  return {
    top: headItem.pos.x + 'px',
    left: headItem.pos.y + 'px',
    height: headItem.pos.h + 'px',
    width: headItem.pos.w + 'px',
  };
}

watch(headItemOriginalStyle, (newStyle, oldStyle) => {
  for( let key in oldStyle ) nowTransitionElement.style[key] = null;
  for( let key in newStyle ) nowTransitionElement.style[key] = newStyle[key];
})

const logRaw = (x: any) => JSON.parse(JSON.stringify(toRaw(x)));

//https://cn.vuejs.org/guide/built-ins/transition.html
function onBeforeEnter(el: Element) {
  console.log('onBeforeEnter', el, logRaw(headItems));
  nowTransitionElement = el;
  headItemOriginalStyle.value = getStyle(nowTransitionItem as HeadItem);
}

function onEnter(el: Element) {
  console.log('onEnter', el, logRaw(headItems));
  setTimeout(()=>{headItemOriginalStyle.value = {}}, 0);
}

function onLeave(el: Element) {
  console.log('onLeave', el, logRaw(headItems));
  nowTransitionElement = el;
  headItemOriginalStyle.value = getStyle(nowTransitionItem as HeadItem);
}

function onAfterLeave(el: Element) {
  console.log('onAfterLeave', el, logRaw(headItems));
  headItemOriginalStyle.value = {};
} 

interface Props {
  minItemWidth: number,
  items: any[]
}

const columns = ref(4);

const props = defineProps<Props>();

const items = reactive(props.items);

const containerRef = ref<InstanceType<typeof HTMLElement>>();

const maxItemCount = ref(20);

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
  overflow-y: scroll;
}

.head-item{
  background:#c99;
  position: absolute; 
  z-index: 9999; 
  top: 0;
  left: 0;
  width: 100%; 
  height: 100%; 
}

/* .head-item.docked{
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
} */
          
.head-item-trans-enter-active,
.head-item-trans-leave-active {
  transition: all 0.5s ease;
}
.head-item-trans-enter-from,
.head-item-trans-leave-to {
}

</style>