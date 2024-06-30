<template>
  <div>
    <pre>{{ show }}</pre>
    <div class="container" :style="{ gridTemplateColumns: `repeat(${columns}, 1fr)` }">
      <div class="item" v-for="item in items" :key="item.id" :ref="setItemEle"
        @click="show = getinfo(itemEles[item.id])"
      >
        {{ item.name }}
      </div>
    </div>
    <div class="controls">
      <label>每行元素数量:</label>
      <input type="number" v-model.number="columns" min="1" max="10" />
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';

let itemEles = [];
function setItemEle(el){
  itemEles.push(el)
}

function getinfo(el) {
  // more position info
  return ' '+el.offsetWidth+' x '+el.offsetHeight
  +' \n'+el.offsetLeft+' x '+el.offsetTop
  +' \n'+el.getBoundingClientRect().left+' x '+el.getBoundingClientRect().top
  +' \n'+el.parentNode.offsetLeft+' x '+el.parentNode.offsetTop
  +' \n'+el.clientLeft+' x '+el.clientTop
  +' \n'+el.scrollLeft+' x '+el.scrollTop
}

let show = ref(123);

let columns = ref(3);

let items = reactive([
        { id: 0, name: 'Item 0' },
        { id: 1, name: 'Item 1' },
        { id: 2, name: 'Item 2' },
        { id: 3, name: 'Item 3' },
        { id: 4, name: 'Item 4' },
        { id: 5, name: 'Item 5' },
        { id: 6, name: 'Item 6' },
        { id: 7, name: 'Item 7' },
        { id: 8, name: 'Item 8' },
        { id: 9, name: 'Item 9' },
      ]);

</script>

<style>
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}

.item {
  background-color: #333;
  padding: 20px;
  text-align: center;
}

.controls {
  margin-top: 20px;
}
</style>