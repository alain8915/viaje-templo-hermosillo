<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
const items = [
  { id:'inicio', label:'Inicio', icon:'bi-house-door' },
  { id:'grupos', label:'Grupos', icon:'bi-people' },
  { id:'programa', label:'Programa', icon:'bi-calendar3' },
  { id:'talleres', label:'Talleres', icon:'bi-stars' },
  { id:'baptisterio', label:'Baptisterio', icon:'bi-water' },
]
const active = ref('inicio')
let observer
onMounted(() => {
  observer = new IntersectionObserver((entries) => {
    const visible = entries.filter(e => e.isIntersecting).sort((a,b) => b.intersectionRatio-a.intersectionRatio)[0]
    if (visible?.target?.id) active.value = visible.target.id
  }, { rootMargin: '-25% 0px -55% 0px', threshold: [0.05,0.2,0.5] })
  items.forEach(i => { const el = document.getElementById(i.id); if (el) observer.observe(el) })
})
onBeforeUnmount(() => observer?.disconnect())
</script>
<template>
  <nav class="bottom-nav d-md-none" aria-label="Navegación móvil">
    <a v-for="item in items" :key="item.id" :href="`#${item.id}`" :class="{ active: active === item.id }">
      <i class="bi" :class="item.icon"></i><small>{{ item.label }}</small>
    </a>
  </nav>
</template>
