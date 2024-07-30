<template>
<div ref="carouselRef" class="carousel-container">
  <LazyComp v-for="(item, idx) in items" :key="idx" :root="carouselRef" root-margin="600px">
    <template #default>
      <slot name="item" :item="item" />
    </template>
  </LazyComp>
</div>
</template>

<script setup lang="ts" generic="T extends object">
import { ref } from 'vue'
import LazyComp from '@/components/LazyComp.vue'

defineProps<{
  items: T[]
  uniqueId: keyof T
}>()

const carouselRef = ref<HTMLDivElement>()
</script>

<style scoped>
.carousel-container {
  display: flex;
  align-items: center;
  overflow-x: scroll;
  scroll-snap-type: x mandatory;
  gap: 16px;
  width: 100%;
  scrollbar-width: none;
}
</style>
