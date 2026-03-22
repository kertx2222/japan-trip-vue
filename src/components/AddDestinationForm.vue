<template>
  <div class="card mb-5">
    <div class="card-content">
      <h2 class="title is-4"> Добавить место для посещения</h2>
      
      <form @submit.prevent="handleSubmit">
        <div class="columns">
          <div class="column is-6">
            <div class="field">
              <label class="label">Название места</label>
              <div class="control has-icons-left">
                <input 
                  v-model="formData.name"
                  class="input" 
                  type="text" 
                  placeholder="Например: Фудзияма"
                  required
                />
                <span class="icon is-small is-left">
                  <i class="fas fa-map-marker-alt"></i>
                </span>
              </div>
            </div>
          </div>
          
          <div class="column is-6">
            <div class="field">
              <label class="label">Город</label>
              <div class="control has-icons-left">
                <input 
                  v-model="formData.city"
                  class="input" 
                  type="text" 
                  placeholder="Например: Токио"
                  required
                />
                <span class="icon is-small is-left">
                  <i class="fas fa-city"></i>
                </span>
              </div>
            </div>
          </div>
        </div>

        <div class="columns">
          <div class="column is-6">
            <div class="field">
              <label class="label">Категория</label>
              <div class="control">
                <div class="select is-fullwidth">
                  <select v-model="formData.category" required>
                    <option value="" disabled>Выберите категорию</option>
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
          </div>
          
          <div class="column is-6">
            <div class="field">
              <label class="label">URL изображения</label>
              <div class="control has-icons-left">
                <input 
                  v-model="formData.imageUrl"
                  class="input" 
                  type="url" 
                  placeholder="https://..."
                />
                <span class="icon is-small is-left">
                  <i class="fas fa-image"></i>
                </span>
              </div>
            </div>
          </div>
        </div>

        <div class="field">
          <label class="label">Описание</label>
          <div class="control">
            <textarea 
              v-model="formData.description"
              class="textarea" 
              placeholder="Почему хотите посетить это место..."
              rows="3"
            ></textarea>
          </div>
        </div>

        <div class="field is-grouped">
          <div class="control">
            <button class="button is-primary" type="submit">
              <span class="icon">
                <i class="fas fa-plus"></i>
              </span>
              <span>Добавить</span>
            </button>
          </div>
          <div class="control">
            <button class="button is-light" type="reset" @click="resetForm">
              <span class="icon">
                <i class="fas fa-undo"></i>
              </span>
              <span>Очистить</span>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-destination'])

const formData = reactive({
  name: '',
  city: '',
  category: '',
  imageUrl: '',
  description: ''
})

const handleSubmit = () => {
  emit('add-destination', { ...formData })
  resetForm()
}

const resetForm = () => {
  formData.name = ''
  formData.city = ''
  formData.category = ''
  formData.imageUrl = ''
  formData.description = ''
}
</script>

<style scoped>
.card {
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}
</style>