<template>
  <div class="virtual-list__container" @scroll="scrollEvent">
    <div class="virtual-list__blank" :style="{height: `${itemHeight * totalCount}px`}"></div>
    <div class="virtual-list__content" :style="{transform: `translate3d(0, ${offsetY}px, 0)`}">
      <div v-for="(item, index) in visibleData" :key="index">{{item.value}}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref, computed} from 'vue';

const containerHeight = 300
const itemHeight = 24
const visibleCount = Math.ceil(containerHeight/itemHeight)

// 模拟数据
const totalCount = ref(10000) // 列表总数
const data = ref(Array.from({length:totalCount.value}).map((_, index) => ({value: index})))

const startIndex = ref(0)
const visibleData = computed(() => ( data.value.slice(startIndex.value, startIndex.value + visibleCount) ))

const offsetY = ref(0)

function scrollEvent(e) {
  const {scrollTop}= e.target
  startIndex.value = Math.floor(scrollTop/itemHeight)
  offsetY.value = scrollTop
}

</script>

<style lang="scss" scoped>
.virtual-list {
  &__container {
    width: 100%;
    height: 300px;
    overflow: auto;
    position: relative;
    border: 1px solid #f66
  }
  &__blank {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    
  }
  &__content {
    transform: translate3d(0,0,0);
  }
}
</style>