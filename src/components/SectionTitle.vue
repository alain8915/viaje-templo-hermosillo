<script setup>
defineProps({ kicker: String, title: String })
</script>
<template>
  <div class="mb-3">
    <div class="section-kicker text-uppercase fw-semibold">{{ kicker }}</div>
    <h2 class="h3 fw-bold mb-0">{{ title }}</h2>
  </div>
</template>
