<template>
  <main>
    <h1>Carousel Assessment</h1>
  <!--  Carousel goes here   -->
    <AppCarousel id="main-carousel" class="main-carousel" :items="photos" unique-id="id">
      <template #item="{item}">
        <div class="photo">
          <h3>{{item.title}}</h3>

          <img class="img-wrapper" :src="item.url" :alt="item.title">
        </div>
      </template>
    </AppCarousel>
  </main>
</template>

<script setup lang="ts">
import { onBeforeMount, ref } from 'vue'
import AppCarousel from '@/components/AppCarousel.vue'

interface Photo {
  albumId: number
  id: number
  thumbnailUrl: string
  url: string
  title: string
}

const photos = ref<Photo[]>([])

function getPhotos () {
  return fetch('https://jsonplaceholder.typicode.com/photos?albumId=1').then((res) =>
      res.json()
        .then(data => { photos.value = data }))
}

onBeforeMount(getPhotos)
</script>

<style scoped>
.main-carousel {
  padding: 16px 0;
}

.main-carousel :deep(.wrapper) {
  width: 33%;
  min-width: 33%;
}

.photo {
  scroll-snap-align: start;
  display: flex;
  flex-direction: column;
  padding: 16px;
  gap: 12px;
  width: 100%;
}

.photo h3 {
  text-transform: capitalize;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.img-wrapper {
  height: 320px;
  width: 100%;
  border-radius: 16px;
}

@media (max-width: 768px) {
  .main-carousel :deep(.wrapper) {
    width: 50%;
    min-width: 50%;
  }
}

@media (max-width: 425px) {
  .main-carousel :deep(.wrapper) {
    width: 100%;
    min-width: 100%;
  }
}
</style>
