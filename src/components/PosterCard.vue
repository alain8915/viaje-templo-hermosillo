<script setup>
defineProps({ src: String, alt: String, label: String, wide: Boolean })
const emit = defineEmits(['open'])
</script>
<template>
  <button class="poster-card card border-0 shadow-sm text-start w-100" :class="{ 'wide-poster': wide }" @click="emit('open', src, alt)">
    <img :src="src" :alt="alt" class="card-img-top" loading="lazy" />
    <div class="card-body py-2 px-3 d-flex align-items-center justify-content-between">
      <strong class="small">{{ label }}</strong>
      <i class="bi bi-arrows-fullscreen"></i>
    </div>
  </button>
</template>
