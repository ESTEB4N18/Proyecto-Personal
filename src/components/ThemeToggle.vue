<script setup>
defineProps({
  isDark: {
    type: Boolean,
    required: true,
  },
})

const emit = defineEmits({
  toggle: () => true,
})
</script>

<template>
  <button
    class="theme-toggle"
    type="button"
    :aria-pressed="isDark"
    :aria-label="isDark ? 'Cambiar a modo claro' : 'Cambiar a modo oscuro'"
    @click="emit('toggle')"
  >
    <span
      class="theme-toggle__icon"
      :class="{ 'theme-toggle__icon--dark': !isDark }"
      aria-hidden="true"
    ></span>
    {{ isDark ? 'Modo claro' : 'Modo oscuro' }}
  </button>
</template>
