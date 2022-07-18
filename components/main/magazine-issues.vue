<template>
  <div
    ref="wrapper"
    class="di-flex fl-wrap issues-wrapper parent-gap jc-center ai-center"
  >
    <div class="col-12 child-gap">
      <slot name="label">
        <div
          class="text-grey-tint-13 te-uppercase te-truncate background-grey-shade-13 child-gap"
        >
          {{ label }}
        </div>
      </slot>
    </div>
    <div class="child-gap" :class="grid ? 'col-5' : 'col-12'" v-if="featured">
      <a-image :src="mainImage.src" class="dimensioned-image"></a-image>
    </div>
    <div
      class="row child-gap parent-gap"
      :class="featured ? 'col-7' : 'col-12'"
      v-if="grid"
    >
      <!-- vue 3 doesn't require keyed v-for, but some lint tools may consider this an error -->
      <div class="col-3 child-gap" v-for="image in gridImages">
        <a-image :src="image.src" class="dimensioned-image"></a-image>
      </div>
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  images: {
    type: Array,
    default: () => [],
  },
  featured: {
    type: Boolean,
    default: true,
  },
  grid: {
    type: Boolean,
    default: false,
  },
  size: {
    type: [String, Number],
    default: '100%',
  },
  label: {
    type: String,
    default: '',
  },
})
// The main image is the image where featured is set to true
const mainImage = computed(
  () =>
    props.images.find((image) => image.featured) ??
    props.images[0] ?? { name: '', src: '', datePublished: '2000-01-01' },
)
// the other images are sorted by date, and with the maximum of 12 images
const gridImages = computed(() =>
  props.images
    .filter((image) => !image.featured)
    .slice(0, 12)
    .sort((a, b) => {
      if (a.datePublished < b.datePublished) {
        return 1
      } else if (a.datePublished > b.datePublished) {
        return -1
      } else {
        return 0
      }
    }),
)

// Aspect ratio is based on the main image
// the other images on the grid should have the same aspect ratio (prevent broken layouts)
let aspectRatio = ref('0.75')
watch(
  mainImage,
  () => {
    if (process.client) {
      let image = new Image()
      image.onload = (img) => {
        aspectRatio.value = (image.width / image.height).toFixed(2)
      }
      image.src = mainImage.value.src
    }
  },
  { immediate: true },
)

// formatted size isn't necessary, just a little more developer friendly
const formattedSize = computed(() =>
  isNaN(props.size) ? props.size : `${props.size}px`,
)

// the gap between elements is 1.5% of the total width
// it's recalculated when the size changes
let gap = ref('4px')
let negativeGap = ref('-4px')
let wrapper = ref(null)
watch(formattedSize, () => {
  if (process.client) {
    setGap()
  }
})
onMounted(() => {
  setGap()
  // if the width is relative, we need to re-calculate it when the window is resized
  if (formattedSize.value.includes('%') || formattedSize.value.includes('vw')) {
    window.addEventListener('resize', () => {
      setGap()
    })
  }
})
function setGap() {
  const width = wrapper.value?.getBoundingClientRect().width
  gap.value = Math.floor(width * 0.01) + 'px'
  negativeGap.value = Math.floor(width * -0.01) + 'px'
}
</script>
<style scoped>
.child-gap {
  padding: v-bind(gap);
}
.parent-gap {
  margin: v-bind(negativeGap);
}
.issues-wrapper {
  width: v-bind(formattedSize);
}
.dimensioned-image {
  width: 100%;
  aspect-ratio: v-bind(aspectRatio);
}
</style>
