<template>
  <div class="box">
    <div class="columns is-vcentered">
      <div class="column is-6">
        <div class="field has-addons">
          <div class="control is-expanded">
            <input 
              v-model="searchQuery"
              class="input" 
              type="text" 
              placeholder=" Поиск по названию или городу..."
            />
          </div>
        </div>
      </div>

      <div class="column is-6">
        <div class="buttons has-addons is-right">
          <button 
            v-for="option in filterOptions" 
            :key="option.value"
            class="button"
            :class="{ 'is-primary': filter === option.value }"
            @click="$emit('update:filter', option.value)"
          >
            {{ option.label }}
          </button>
        </div>
      </div>
    </div>

    <div class="tags has-addons">
      <span class="tag is-dark">Всего</span>
      <span class="tag is-primary">{{ total }}</span>
      
      <span class="tag is-dark ml-2">Посещено</span>
      <span class="tag is-success">{{ visited }}</span>
      
      <span class="tag is-dark ml-2">В плане</span>
      <span class="tag is-warning">{{ planned }}</span>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'destinations'])
const emit = defineEmits(['update:filter', 'update:search'])

const searchQuery = computed({
  get: () => props.search || '',
  set: (value) => emit('update:search', value)
})

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'planned', label: 'В плане' },
  { value: 'visited', label: 'Посещено' }
]

const total = computed(() => props.destinations.length)
const visited = computed(() => props.destinations.filter(d => d.visited).length)
const planned = computed(() => props.destinations.filter(d => !d.visited).length)
</script>

<style scoped>
.ml-2 {
  margin-left: 0.5rem;
}
</style>