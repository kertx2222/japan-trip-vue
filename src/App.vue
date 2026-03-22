<template>
  <div class="app">
    <nav class="navbar is-primary" role="navigation" aria-label="main navigation">
      <div class="container">
        <div class="navbar-brand">
          <a class="navbar-item has-text-weight-bold is-size-4" href="#">
            🇯🇵 JapanTrip Planner
          </a>
        </div>
        <div class="navbar-menu">
          <div class="navbar-end">
            <a href="#" class="navbar-item has-text-white">Главная</a>
            <a href="#" class="navbar-item has-text-white">Маршруты</a>
            <a href="#" class="navbar-item has-text-white">Контакты</a>
          </div>
        </div>
      </div>
    </nav>

    <section class="hero is-primary is-medium">
      <div class="hero-body">
        <div class="container has-text-centered">
          <h1 class="title is-1 has-text-white">Планировщик путешествия по Японии</h1>
          <p class="subtitle is-4 has-text-white">Добавляйте места, которые хотите посетить</p>
        </div>
      </div>
    </section>

    <section class="section">
      <div class="container">
        
        <AddDestinationForm @add-destination="addDestination" />

        <DestinationFilters 
          v-model:filter="currentFilter"
          v-model:search="searchQuery"
          :destinations="destinations"
        />

        <div v-if="filteredDestinations.length === 0" class="has-text-centered mt-6">
          <p class="is-size-4 has-text-grey">Мест пока нет 😔</p>
          <p class="has-text-grey">Добавьте первое место для посещения!</p>
        </div>

        <div v-else class="columns is-multiline mt-5">
          <div 
            v-for="destination in filteredDestinations" 
            :key="destination.id"
            class="column is-4"
          >
            <DestinationCard 
              :destination="destination"
              @toggle="toggleDestination"
              @delete="deleteDestination"
              @edit="openEditModal"
            />
          </div>
        </div>

        <DestinationModal 
          v-if="showEditModal"
          :destination="editingDestination"
          @save="saveEdit"
          @close="closeEditModal"
        />

      </div>
    </section>

    <footer class="footer has-background-primary">
      <div class="content has-text-centered has-text-white">
        <p><strong>JapanTrip Planner</strong> — Практическое 15</p>
        <p>Vue 3 + Bulma CSS</p>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddDestinationForm from './components/AddDestinationForm.vue'
import DestinationFilters from './components/DestinationFilters.vue'
import DestinationCard from './components/DestinationCard.vue'
import DestinationModal from './components/DestinationModal.vue'

const destinations = ref([])
const currentFilter = ref('all')
const searchQuery = ref('')
const showEditModal = ref(false)
const editingDestination = ref(null)

const savedDestinations = localStorage.getItem('japanDestinations')
if (savedDestinations) {
  destinations.value = JSON.parse(savedDestinations)
}

watch(destinations, (newDestinations) => {
  localStorage.setItem('japanDestinations', JSON.stringify(newDestinations))
}, { deep: true })

const addDestination = (destinationData) => {
  const newDestination = {
    id: Date.now(),
    ...destinationData,
    visited: false,
    rating: 0,
    createdAt: new Date().toISOString()
  }
  destinations.value.push(newDestination)
}

const toggleDestination = (id) => {
  const destination = destinations.value.find(d => d.id === id)
  if (destination) {
    destination.visited = !destination.visited
    if (!destination.visited) {
      destination.rating = 0
    }
  }
}

const deleteDestination = (id) => {
  if (confirm('Удалить это место из списка?')) {
    destinations.value = destinations.value.filter(d => d.id !== id)
  }
}

const openEditModal = (destination) => {
  editingDestination.value = { ...destination }
  showEditModal.value = true
}

const closeEditModal = () => {
  showEditModal.value = false
  editingDestination.value = null
}

const saveEdit = (updatedDestination) => {
  const index = destinations.value.findIndex(d => d.id === updatedDestination.id)
  if (index !== -1) {
    destinations.value[index] = updatedDestination
  }
  closeEditModal()
}

const filteredDestinations = computed(() => {
  return destinations.value
    .filter(destination => {
      if (currentFilter.value === 'visited') return destination.visited
      if (currentFilter.value === 'planned') return !destination.visited
      return true
    })
    .filter(destination => {
      if (!searchQuery.value) return true
      const query = searchQuery.value.toLowerCase()
      return destination.name.toLowerCase().includes(query) ||
             destination.city.toLowerCase().includes(query)
    })
})
</script>

<style>
.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.section {
  flex: 1;
}

.mt-5 {
  margin-top: 2rem;
}

.mt-6 {
  margin-top: 3rem;
}

.card {
  transition: transform 0.3s, box-shadow 0.3s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.15);
}

.rating-stars {
  color: #ffc107;
  font-size: 1.2rem;
  cursor: pointer;
}

.rating-stars span:hover {
  transform: scale(1.2);
}
</style>