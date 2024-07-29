<template>
  <main>
    <h1>Carousel Assessment</h1>
  <!--  Carousel goes here   -->
    <AppCarousel id="main-carousel" class="main-carousel" :items="photos" unique-id="id">
      <template #item="{item}">
        <div class="photo">
          <h3>{{item.title}}</h3>

          <LazyImage class="img-wrapper" :src="item.url" :alt="item.title" img-class="img" />
        </div>
      </template>
    </AppCarousel>
  </main>
</template>

<script setup lang="ts">
import { onBeforeMount, ref } from 'vue'
import AppCarousel from '@/components/carousel/AppCarousel.vue'
import LazyImage from '@/components/carousel/LazyImage.vue'

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

.photo {
  scroll-snap-align: start;
  display: flex;
  flex-direction: column;
  padding: 16px;
  gap: 12px;
  width: 33%;
  min-width: 320px;
}

.photo h3 {
  text-transform: capitalize;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.img-wrapper {
  height: 320px;
}

@media (max-width: 768px) {
  .photo {
    width: 50%;
  }
}

@media (max-width: 425px) {
  .photo {
    width: 100%;
    min-width: 100%;
  }
}
</style>
