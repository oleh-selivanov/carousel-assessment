<template>
  <LazyComp>
    <template #default>
      <img :class="imgClass" ref="img" :src="src" :alt="alt">
    </template>
  </LazyComp>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import LazyComp from '@/components/LazyComp.vue'

defineProps<{
  alt?: string
  src?: string
  imgClass?: string
}>()
const img = ref<HTMLImageElement | null>(null)
</script>

<style scoped>
img {
  width: 100%;
  height: 100%;
  border-radius: 16px;
}
</style>
