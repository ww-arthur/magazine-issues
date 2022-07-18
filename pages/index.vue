<template>
  <div class="po-relative oy-auto">
    <div
      class="issues-controls pa-5 row ai-center ro-bottom-5 background-primary-blend-7"
    >
      <div class="col-2 fw-6">Controls</div>
      <div class="col-4 px-2">
        <a-slider label="Size" max="1080" v-model="size">{{ size }}px</a-slider>
      </div>
      <div class="col-1 px-2">
        <label class="fs-3 di-flex ai-center">
          <input v-model="grid" type="checkbox" />
          Grid
        </label>
      </div>
      <div class="col-1 px-2">
        <label class="fs-3 di-flex ai-center">
          <input v-model="featured" type="checkbox" />
          Featured
        </label>
      </div>
      <div class="col-4 di-flex jc-end">
        <a-button template="glassy" @click="darkMode = !darkMode">
          {{ darkMode ? 'Light' : 'Dark' }}
        </a-button>
      </div>
    </div>

    <div class="row mt-10 jc-center ai-center">
      <div class="col-12 md:col-10 lg-col-8 di-flex jc-center">
        <main-magazine-issues
          label="Time magazine"
          :images="magazineIssues"
          :size="size"
          :grid="grid"
          :featured="featured"
        ></main-magazine-issues>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
let magazineIssues = useMagazineIssues()
let size = ref(250)
let grid = ref(true)
let featured = ref(true)

// theme variables
let darkMode = useDarkmode()
</script>
<style>
.issues-controls {
  position: sticky;
  z-index: 2;
  top: 0;
  backdrop-filter: blur(10px);
}
</style>
