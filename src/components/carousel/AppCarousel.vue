<template>
<div class="carousel-container">
    <slot v-for="(item, idx) in items" :key="idx" name="item" :item="item" />
</div>
</template>

<script setup lang="ts" generic="T extends object">
defineProps<{
  items: T[]
  uniqueId: keyof T
}>()
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
