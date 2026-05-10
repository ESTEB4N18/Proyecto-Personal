<script setup>
defineProps({
  categories: {
    type: Array,
    required: true,
    validator: (value) => value.every((category) => typeof category === 'string'),
  },
  selectedCategory: {
    type: String,
    required: true,
  },
})

const emit = defineEmits({
  'update:selectedCategory': (category) => typeof category === 'string',
})
</script>

<template>
  <div class="category-filter" aria-label="Filtrar por categoria">
    <span class="category-filter__label">Categoria</span>
    <div class="category-filter__options">
      <button
        v-for="category in categories"
        :key="category"
        class="chip"
        :class="{ 'chip--active': selectedCategory === category }"
        :aria-pressed="selectedCategory === category"
        type="button"
        @click="emit('update:selectedCategory', category)"
      >
        {{ category }}
      </button>
    </div>
  </div>
</template>
