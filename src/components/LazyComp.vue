<template>
  <div class="wrapper" ref="wrapper">
    <slot v-if="loaded" :onLoad="onLoad" />
    <slot v-else name="loading" />
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

const emit = defineEmits(['load'])

const props = defineProps<{
  threshold?: number
  rootMargin?: string
  root?: Element
}>()

const observer = ref<IntersectionObserver | null>(null)
const wrapper = ref<HTMLDivElement | null>(null)

const loaded = ref(false)

onMounted(() => {
  if (window.IntersectionObserver) {
    const handleIntersect: IntersectionObserverCallback = (entries, observer) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          onLoad()
          if (wrapper.value) {
            observer.unobserve(wrapper.value)
          }
        }
      })
    }

    nextTick(() => {
      observer.value = new IntersectionObserver(handleIntersect, {
        root: props.root,
        threshold: props.threshold,
        rootMargin: props.rootMargin
      })

      if (wrapper.value) {
        observer.value.observe(wrapper.value)
      }
    })
  } else {
    onLoad()
  }
})

onUnmounted(() => {
  if (wrapper.value && observer.value) {
    observer.value.unobserve(wrapper.value)
  }
})

function onLoad () {
  loaded.value = true
  emit('load')
}
</script>

<style scoped>
.wrapper {
  position: relative;
  width: 100%;
  height: 100%;
}
</style>
