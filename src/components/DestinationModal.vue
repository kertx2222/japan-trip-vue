<template>
  <div class="modal is-active">
    <div class="modal-background" @click="$emit('close')"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title"> Редактировать место</p>
        <button class="delete" @click="$emit('close')"></button>
      </header>
      
      <section class="modal-card-body">
        <form @submit.prevent="handleSubmit">
          <div class="field">
            <label class="label">Название</label>
            <div class="control">
              <input 
                v-model="formData.name"
                class="input" 
                type="text" 
                required
              />
            </div>
          </div>

          <div class="field">
            <label class="label">Город</label>
            <div class="control">
              <input 
                v-model="formData.city"
                class="input" 
                type="text" 
                required
              />
            </div>
          </div>

          <div class="field">
            <label class="label">Категория</label>
            <div class="control">
              <div class="select is-fullwidth">
                <select v-model="formData.category">
                  <option value="храм"> Храм</option>
                  <option value="природа"> Природа</option>
                  <option value="музей"> Музей</option>
                  <option value="еда"> Еда</option>
                  <option value="шопинг"> Шопинг</option>
                  <option value="развлечения"> Развлечения</option>
                </select>
              </div>
            </div>
          </div>

          <div class="field">
            <label class="label">Описание</label>
            <div class="control">
              <textarea 
                v-model="formData.description"
                class="textarea" 
                rows="3"
              ></textarea>
            </div>
          </div>
        </form>
      </section>
      
      <footer class="modal-card-foot">
        <button class="button is-primary" @click="handleSubmit">Сохранить</button>
        <button class="button" @click="$emit('close')">Отмена</button>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { reactive, watch } from 'vue'

const props = defineProps(['destination'])
const emit = defineEmits(['save', 'close'])

const formData = reactive({
  id: '',
  name: '',
  city: '',
  category: '',
  description: ''
})

watch(() => props.destination, (newDest) => {
  if (newDest) {
    formData.id = newDest.id
    formData.name = newDest.name
    formData.city = newDest.city
    formData.category = newDest.category
    formData.description = newDest.description || ''
  }
}, { immediate: true })

const handleSubmit = () => {
  emit('save', { ...formData })
}
</script>

<style scoped>
</style>