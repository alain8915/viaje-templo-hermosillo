<script setup>
import { onMounted, onBeforeUnmount, ref, watch } from 'vue'
const props = defineProps({ src: String, alt: String })
const emit = defineEmits(['close'])
const dialog = ref(null)

watch(() => props.src, (value) => {
  if (!dialog.value) return
  if (value && !dialog.value.open) dialog.value.showModal()
  if (!value && dialog.value.open) dialog.value.close()
})

function close() {
  if (dialog.value?.open) dialog.value.close()
  emit('close')
}
function onCancel(e) { e.preventDefault(); close() }
onMounted(() => dialog.value?.addEventListener('cancel', onCancel))
onBeforeUnmount(() => dialog.value?.removeEventListener('cancel', onCancel))
</script>
<template>
  <dialog ref="dialog" class="image-modal" @click.self="close">
    <button class="btn btn-light rounded-circle modal-close" aria-label="Cerrar" @click="close"><i class="bi bi-x-lg"></i></button>
    <img v-if="src" :src="src" :alt="alt" />
  </dialog>
</template>
