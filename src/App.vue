<script setup>
import { computed, ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import BottomNav from './components/BottomNav.vue'
import ImageModal from './components/ImageModal.vue'
import PosterCard from './components/PosterCard.vue'
import SectionTitle from './components/SectionTitle.vue'
import { groups, workshops, triviaRole } from './data/tripData'

const activeGroupId = ref(1)
const activeGroup = computed(() => groups.find(g => g.id === activeGroupId.value))
const modal = ref({ src:'', alt:'' })
function openImage(src, alt='Infografía ampliada') { modal.value = { src, alt } }
function closeImage() { modal.value = { src:'', alt:'' } }
</script>

<template>
  <AppHeader />

  <main class="pb-5">
    <section class="container mt-n3 position-relative z-2">
      <div class="app-card overview-card shadow-sm">
        <SectionTitle kicker="Información general" title="Datos principales" />
        <div class="row g-2">
          <div class="col-6 col-lg-3" v-for="item in [
            ['Salida','Jueves 19 · noche','bi-bus-front'],
            ['Regreso','Sábado 21 · 5:00 p. m.','bi-arrow-return-left'],
            ['Destino','Templo de Hermosillo','bi-building'],
            ['Jóvenes','38 confirmados','bi-people']
          ]" :key="item[0]">
            <div class="info-stat h-100"><i class="bi" :class="item[2]"></i><span>{{ item[0] }}</span><strong>{{ item[1] }}</strong></div>
          </div>
        </div>
      </div>
    </section>

    <section id="grupos" class="container section-anchor py-4">
      <SectionTitle kicker="Mi grupo" title="Consulta rápida por color" />
      <div class="group-shortcuts d-grid gap-2 mb-3" style="grid-template-columns:repeat(4,1fr)">
        <button v-for="group in groups" :key="group.id" class="btn group-chip" :class="{ active: activeGroupId === group.id }" :style="{ '--group': group.color }" @click="activeGroupId = group.id">
          <span class="group-dot"></span>G{{ group.id }}
        </button>
      </div>

      <transition name="fade" mode="out-in">
        <article :key="activeGroup.id" class="app-card group-panel shadow-sm" :style="{ '--group': activeGroup.color }">
          <div class="d-flex justify-content-between gap-3 align-items-start flex-wrap mb-3">
            <div><span class="group-badge">{{ activeGroup.name }} · {{ activeGroup.label }}</span><h3 class="h4 fw-bold mt-2 mb-0">{{ activeGroup.members.length }} jóvenes</h3></div>
            <div class="session-stack"><span>Vie {{ activeGroup.friday }}</span><span>Sáb {{ activeGroup.saturday }}</span></div>
          </div>
          <ol class="people-list mb-3">
            <li v-for="member in activeGroup.members" :key="member">{{ member }}</li>
          </ol>
          <button class="btn btn-outline-primary w-100" @click="openImage(activeGroup.poster, `Infografía ${activeGroup.name}`)"><i class="bi bi-image me-2"></i>Ver infografía del {{ activeGroup.name }}</button>
        </article>
      </transition>
    </section>

    <section id="programa" class="container section-anchor py-4">
      <SectionTitle kicker="Programa" title="Itinerario y rotaciones" />
      <div class="row g-3 mb-3">
        <div class="col-md-6"><article class="app-card h-100"><h3 class="h5 fw-bold">Viernes 20</h3><p>Sesiones: <strong>7:00 a. m., 9:00 a. m., 5:00 p. m. y 7:00 p. m.</strong></p><p class="mb-0">Hospedaje solo la noche del viernes.</p></article></div>
        <div class="col-md-6"><article class="app-card h-100"><h3 class="h5 fw-bold">Sábado 21</h3><p>Sesiones: <strong>7:00 a. m., 9:00 a. m., 11:00 a. m. y 1:00 p. m.</strong></p><p class="mb-0">Regreso en camiones a las <strong>5:00 p. m.</strong></p></article></div>
      </div>
      <div class="row g-3">
        <div class="col-md-6"><PosterCard src="/itinerario.png" alt="Itinerario visual del viaje al templo" label="Itinerario visual" @open="openImage" /></div>
        <div class="col-md-6"><PosterCard src="/rol-bloques-sesiones.png" alt="Rol de bloques y sesiones" label="Rol de bloques y sesiones" @open="openImage" /></div>
      </div>
    </section>

    <section id="talleres" class="container section-anchor py-4">
      <SectionTitle kicker="Actividades" title="Talleres" />
      <div class="row g-3">
        <div v-for="workshop in workshops" :key="workshop.title" class="col-12 col-md-6 col-lg-4">
          <article class="app-card workshop-card h-100">
            <i class="bi workshop-icon" :class="workshop.icon"></i>
            <h3 class="h5 fw-bold">{{ workshop.title }}</h3>
            <p class="small fw-semibold text-primary mb-2">{{ workshop.owner }} · {{ workshop.day }}</p>
            <p class="mb-0">{{ workshop.description }}</p>
          </article>
        </div>
      </div>
      <div class="mt-3"><PosterCard src="/bloques-por-barrio.png" alt="Asignación de bloques por barrio" label="Ver infografía de talleres" wide @open="openImage" /></div>

      <article class="app-card mt-3">
        <h3 class="h5 fw-bold mb-3">Rol de Trivia</h3>
        <div class="role-list">
          <div v-for="slot in triviaRole" :key="slot.time" class="role-row">
            <time>{{ slot.time }}</time>
            <span><strong>{{ slot.leader }}</strong><small>{{ slot.helper }}</small></span>
          </div>
        </div>
      </article>
    </section>

    <section id="baptisterio" class="container section-anchor py-4">
      <SectionTitle kicker="Servicio" title="Asignaciones del baptisterio" />
      <p class="text-secondary">Toca una infografía para verla en pantalla completa.</p>
      <div class="row g-3">
        <div class="col-md-6"><PosterCard src="/asignaciones-baptismos-viernes.png" alt="Asignaciones de sesiones de baptismos del viernes" label="Viernes 20" @open="openImage" /></div>
        <div class="col-md-6"><PosterCard src="/asignaciones-baptismos-sabado.png" alt="Asignaciones de sesiones de baptismos del sábado" label="Sábado 21" @open="openImage" /></div>
      </div>
    </section>

    <section id="preparacion" class="container py-4">
      <SectionTitle kicker="Antes de salir" title="Preparación" />
      <div class="row g-3">
        <div v-for="item in [
          ['bi-tree','Nombres familiares','Llevar nombres preparados para las ordenanzas del templo.'],
          ['bi-person-standing-dress','Ropa','Ropa apropiada para el templo y lo necesario para hospedaje y traslado.'],
          ['bi-droplet','Hidratación','Llevar agua y seguir las indicaciones durante traslados y actividades.'],
          ['bi-circle','Pulsera','Usar el color asignado: azul, verde, naranja o morado.']
        ]" :key="item[1]" class="col-6 col-lg-3">
          <article class="app-card prep-card h-100"><i class="bi" :class="item[0]"></i><h3 class="h6 fw-bold">{{ item[1] }}</h3><p class="small mb-0">{{ item[2] }}</p></article>
        </div>
      </div>
    </section>

    <section id="faq" class="container py-4 mb-5">
      <SectionTitle kicker="Ayuda" title="Preguntas frecuentes" />
      <div class="accordion shadow-sm rounded-4 overflow-hidden" id="faqAccordion">
        <div class="accordion-item" v-for="(faq, idx) in [
          ['¿Cuántos jóvenes hay en cada grupo?','Grupo 1 y Grupo 2 tienen 10 jóvenes. Grupo 3 y Grupo 4 tienen 9.'],
          ['¿Cuántas sesiones tendrá cada grupo?','Cada grupo participa en una sesión de baptismos el viernes y otra el sábado.'],
          ['¿Para qué sirven las pulseras?','Permiten identificar rápidamente a cada grupo durante sesiones, talleres y traslados.'],
          ['¿Quién coordina transporte, hospedaje y comidas?','La logística general está a cargo de la Presidencia de Estaca.']
        ]" :key="faq[0]">
          <h2 class="accordion-header"><button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" :data-bs-target="`#faq-${idx}`">{{ faq[0] }}</button></h2>
          <div :id="`faq-${idx}`" class="accordion-collapse collapse" data-bs-parent="#faqAccordion"><div class="accordion-body">{{ faq[1] }}</div></div>
        </div>
      </div>
    </section>
  </main>

  <footer class="site-footer py-4">
    <div class="container text-center"><strong class="d-block">Viaje al Templo de Hermosillo 2026</strong><span class="d-block">Estaca Culiacán México Humaya</span><small>Última actualización: 17 de noviembre de 2026</small></div>
  </footer>

  <BottomNav />
  <ImageModal :src="modal.src" :alt="modal.alt" @close="closeImage" />
</template>
