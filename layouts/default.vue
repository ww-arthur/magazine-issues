<template>
  <div class="a-app" :class="darkMode ? `dark` : ''">
    <a-div
      :colors="{
        default: 'text-primary-shade-9 background-grey-tint-13',
        dark: 'background-grey-shade-13 text-grey-tint-15',
      }"
      class="di-flex fl-column he-min-100vh he-max-100vh ov-hidden"
    >
      <slot></slot>
    </a-div>
  </div>
</template>
<script setup lang="ts">
let darkMode = useDarkmode()
watch(darkMode, (dark) => {
  localStorage.setItem('theme', dark ? 'dark' : 'light')
})
onMounted(() => {
  const theme = localStorage.getItem('theme')
  if (theme) {
    if (theme === 'dark') {
      darkMode.value = true
    } else {
      darkMode.value = false
    }
  } else if (
    window.matchMedia &&
    window.matchMedia('(prefers-color-scheme: dark)').matches
  ) {
    darkMode.value = true
    localStorage.setItem('theme', 'dark')
  }
})
</script>
<style></style>
