<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  destination: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits({
  select: (destination) => Boolean(destination?.id),
})

const imageError = ref(false)

watch(
  () => props.destination.imagen,
  () => {
    imageError.value = false
  },
)
</script>

<template>
  <article class="destination-card">
    <div class="destination-card__media">
      <img
        v-if="!imageError"
        :src="destination.imagen"
        :alt="`Vista representativa de ${destination.nombre}`"
        loading="lazy"
        @error="imageError = true"
      />
      <div v-else class="destination-card__fallback" aria-hidden="true">
        {{ destination.nombre.charAt(0) }}
      </div>
      <span class="destination-card__badge">{{ destination.categoria }}</span>
    </div>

    <div class="destination-card__body">
      <div>
        <h2>{{ destination.nombre }}</h2>
        <p>{{ destination.provincia }}</p>
      </div>
      <button type="button" class="button button--primary" @click="emit('select', destination)">
        Ver mas
      </button>
    </div>
  </article>
</template>
