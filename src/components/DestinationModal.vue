<script setup>
import { onBeforeUnmount, onMounted } from 'vue'
import AudioPlayer from './AudioPlayer.vue'

const props = defineProps({
  destination: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits({
  close: () => true,
})

const closeModal = () => emit('close')

const handleKeydown = (event) => {
  if (event.key === 'Escape') {
    closeModal()
  }
}

onMounted(() => {
  document.addEventListener('keydown', handleKeydown)
})

onBeforeUnmount(() => {
  document.removeEventListener('keydown', handleKeydown)
})
</script>

<template>
  <div class="modal-backdrop" role="presentation" @click.self="closeModal">
    <section
      class="destination-modal"
      role="dialog"
      aria-modal="true"
      :aria-labelledby="`modal-title-${props.destination.id}`"
    >
      <button
        class="modal-close"
        type="button"
        aria-label="Cerrar detalle del destino"
        @click="closeModal"
      >
        &times;
      </button>

      <div class="destination-modal__image">
        <img :src="destination.imagen" :alt="`Imagen de ${destination.nombre}`" />
      </div>

      <div class="destination-modal__content">
        <span class="destination-modal__category">{{ destination.categoria }}</span>
        <h2 :id="`modal-title-${destination.id}`">{{ destination.nombre }}</h2>
        <p class="destination-modal__province">{{ destination.provincia }}</p>
        <p class="destination-modal__description">{{ destination.descripcion }}</p>

        <div class="curiosities">
          <h3>Datos curiosos</h3>
          <ul>
            <li v-for="curiosity in destination.datosCuriosos" :key="curiosity">
              {{ curiosity }}
            </li>
          </ul>
        </div>

        <AudioPlayer
          v-if="destination.audio"
          :src="destination.audio"
          :title="`Audio descriptivo de ${destination.nombre}`"
        />
      </div>
    </section>
  </div>
</template>
