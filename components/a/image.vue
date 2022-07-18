<template>
  <div class="image po-relative">
    <slot />
    <div v-if="loading" class="po-absolute to-50 wi-100">
      <a-bar :loading="loading" height="4px" color="primary-blend-6"></a-bar>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  src: {
    type: String,
    default: '',
  },
})
let loading = ref(true)
watch(
  () => props.src,
  () => {
    if (process.client) {
      let image = new Image()
      image.onload = (img) => {
        loading.value = false
      }
      image.src = props.src
    }
  },
  { immediate: true },
)
let imageUrl = computed(() => `url(${props.src})`)
</script>
<style scoped>
.image {
  background-image: v-bind(imageUrl) !important;
  background-size: 100%;
}
</style>
