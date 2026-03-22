<template>
  <div class="card" :class="{ 'is-success': destination.visited }">
    <div class="card-image" v-if="destination.imageUrl">
      <figure class="image is-4by3">
        <img :src="destination.imageUrl" :alt="destination.name" />
      </figure>
    </div>
    
    <div class="card-content">
      <div class="media">
        <div class="media-content">
          <p class="title is-5">{{ destination.name }}</p>
          <p class="subtitle is-6 has-text-grey">
            <span class="icon"><i class="fas fa-city"></i></span>
            {{ destination.city }}
          </p>
          <span class="tag is-primary">{{ destination.category }}</span>
          <span 
            class="tag ml-2" 
            :class="destination.visited ? 'is-success' : 'is-warning'"
          >
            {{ destination.visited ? ' Посещено' : ' Запланировано' }}
          </span>
        </div>
      </div>

      <div class="content mt-3" v-if="destination.description">
        <p class="is-size-7">{{ destination.description }}</p>
      </div>

      <div class="content mt-3" v-if="destination.visited">
        <label class="label is-size-7">Оценка:</label>
        <div class="rating-stars">
          <span 
            v-for="star in 5" 
            :key="star"
            @click="setRating(star)"
          >
            {{ star <= destination.rating ? '★' : '☆' }}
          </span>
          <span class="ml-2 has-text-grey is-size-7">({{ destination.rating }}/5)</span>
        </div>
      </div>

      <div class="buttons are-small mt-4">
        <button 
          class="button is-primary is-light" 
          @click="$emit('toggle')"
        >
          {{ destination.visited ? 'Вернуть в план' : 'Отметить посещённым' }}
        </button>
        <button 
          class="button is-info is-light" 
          @click="$emit('edit', destination)"
        >
          <span class="icon"><i class="fas fa-edit"></i></span>
        </button>
        <button 
          class="button is-danger is-light" 
          @click="$emit('delete')"
        >
          <span class="icon"><i class="fas fa-trash"></i></span>
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(['destination'])
const emit = defineEmits(['toggle', 'delete', 'edit', 'rate'])

const setRating = (rating) => {
  emit('rate', props.destination.id, rating)
}
</script>

<style scoped>
.card {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.card-content {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.buttons {
  margin-top: auto;
}

.rating-stars span {
  cursor: pointer;
  transition: transform 0.2s;
}

.rating-stars span:hover {
  transform: scale(1.3);
}

.ml-2 {
  margin-left: 0.5rem;
}

.mt-3 {
  margin-top: 0.75rem;
}

.mt-4 {
  margin-top: 1rem;
}
</style>