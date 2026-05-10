<script setup>
import { computed } from 'vue'

const props = defineProps({
  modelValue: {
    type: String,
    default: '',
  },
})

const emit = defineEmits({
  'update:modelValue': (value) => typeof value === 'string',
})

const searchText = computed({
  get: () => props.modelValue,
  set: (value) => emit('update:modelValue', value),
})
</script>

<template>
  <label class="search-field">
    <span>Buscar por destino o provincia</span>
    <input
      v-model="searchText"
      type="search"
      placeholder="Ejemplo: Puntarenas, playa, Monteverde..."
      autocomplete="off"
    />
  </label>
</template>
