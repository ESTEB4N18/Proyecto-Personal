<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import destinosUrl from './data/destinos.json?url'
import AppHeader from './components/Header.vue'
import SearchBar from './components/SearchBar.vue'
import CategoryFilter from './components/CategoryFilter.vue'
import DestinationCard from './components/DestinationCard.vue'
import DestinationModal from './components/DestinationModal.vue'
import ThemeToggle from './components/ThemeToggle.vue'

const destinos = ref([])
const loading = ref(true)
const errorMessage = ref('')
const searchQuery = ref('')
const selectedCategory = ref('Todas')
const selectedDestination = ref(null)
const isDarkMode = ref(false)

const normalizeText = (value) => value.toString().toLowerCase().trim()

const categories = computed(() => {
  const uniqueCategories = new Set(destinos.value.map((destino) => destino.categoria))
  return ['Todas', ...Array.from(uniqueCategories).sort()]
})

const filteredDestinations = computed(() => {
  const query = normalizeText(searchQuery.value)

  return destinos.value.filter((destino) => {
    const matchesSearch =
      normalizeText(destino.nombre).includes(query) ||
      normalizeText(destino.provincia).includes(query)
    const matchesCategory =
      selectedCategory.value === 'Todas' || destino.categoria === selectedCategory.value

    return matchesSearch && matchesCategory
  })
})

const applyTheme = () => {
  document.documentElement.dataset.theme = isDarkMode.value ? 'dark' : 'light'
}

const initializeTheme = () => {
  const savedTheme = localStorage.getItem('ucr-destinos-theme')

  if (savedTheme) {
    isDarkMode.value = savedTheme === 'dark'
  } else {
    isDarkMode.value = window.matchMedia('(prefers-color-scheme: dark)').matches
  }

  applyTheme()
}

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value
}

const loadDestinations = async () => {
  loading.value = true
  errorMessage.value = ''

  try {
    // Vite resuelve esta URL tanto en desarrollo como en la version compilada.
    const response = await fetch(destinosUrl)

    if (!response.ok) {
      throw new Error('No se pudo cargar el archivo de destinos.')
    }

    destinos.value = await response.json()
  } catch (error) {
    errorMessage.value = error.message
  } finally {
    loading.value = false
  }
}

const openDestination = (destination) => {
  selectedDestination.value = destination
}

const closeDestination = () => {
  selectedDestination.value = null
}

watch(isDarkMode, () => {
  localStorage.setItem('ucr-destinos-theme', isDarkMode.value ? 'dark' : 'light')
  applyTheme()
})

watch(selectedDestination, (destination) => {
  document.body.style.overflow = destination ? 'hidden' : ''
})

onMounted(() => {
  initializeTheme()
  loadDestinations()
})
</script>

<template>
  <div class="app-shell">
    <div class="top-actions" aria-label="Preferencias de interfaz">
      <ThemeToggle :is-dark="isDarkMode" @toggle="toggleTheme" />
    </div>

    <AppHeader
      title="Destinos Turisticos de Costa Rica"
      subtitle="Enciclopedia interactiva multimedia para explorar ciudades, playas, montanas, volcanes y parques nacionales del pais."
    />

    <main class="content" aria-live="polite">
      <section class="controls-panel" aria-label="Busqueda y filtros">
        <SearchBar v-model="searchQuery" />
        <CategoryFilter
          :categories="categories"
          :selected-category="selectedCategory"
          @update:selected-category="selectedCategory = $event"
        />
      </section>

      <section class="results-summary" aria-label="Resumen de resultados">
        <p>
          <strong>{{ filteredDestinations.length }}</strong>
          destino{{ filteredDestinations.length === 1 ? '' : 's' }} encontrado{{
            filteredDestinations.length === 1 ? '' : 's'
          }}
        </p>
      </section>

      <section v-if="loading" class="state-message" aria-label="Cargando destinos">
        Cargando destinos turisticos...
      </section>

      <section v-else-if="errorMessage" class="state-message state-message--error" role="alert">
        {{ errorMessage }}
      </section>

      <section v-else-if="filteredDestinations.length" class="destinations-grid">
        <DestinationCard
          v-for="destination in filteredDestinations"
          :key="destination.id"
          :destination="destination"
          @select="openDestination"
        />
      </section>

      <section v-else class="state-message">
        No hay destinos que coincidan con la busqueda y categoria seleccionadas.
      </section>
    </main>

    <DestinationModal
      v-if="selectedDestination"
      :destination="selectedDestination"
      @close="closeDestination"
    />
  </div>
</template>
