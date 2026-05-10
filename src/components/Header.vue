<script setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
  subtitle: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <header class="hero">
    <div class="hero__content">
      <span class="hero__eyebrow">IF7102 Multimedios - Universidad de Costa Rica</span>
      <h1>{{ title }}</h1>
      <p>{{ subtitle }}</p>
    </div>
  </header>
</template>
