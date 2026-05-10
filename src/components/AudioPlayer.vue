<script setup>
defineProps({
  src: {
    type: String,
    required: true,
  },
  title: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="audio-player">
    <p>{{ title }}</p>
    <audio :src="src" controls preload="metadata">
      Tu navegador no soporta el elemento de audio HTML5.
    </audio>
  </div>
</template>
