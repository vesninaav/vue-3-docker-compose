<template>
  <div class="leaderboard">
    <h3>Таблица лидеров</h3>
    <div v-if="loading">Загрузка...</div>
    <div v-else-if="error">Ошибка: {{ error }}</div>
    <ul v-else>
      <li v-for="user in users" :key="user.id">
        {{ user.name }}: {{ user.score }} элементов
      </li>
    </ul>
    <button @click="fetchLeaderboard" :disabled="loading">
      {{ loading ? 'Загрузка...' : 'Обновить' }}
    </button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const users = ref([])
    const loading = ref(false)
    const error = ref(null)

    const fetchLeaderboard = async () => {
      loading.value = true
      error.value = null
      
      try {
        // Имитация запроса к API
        const response = await new Promise(resolve => 
          setTimeout(() => resolve({
            ok: true,
            json: () => Promise.resolve([
              { id: 1, name: 'Алхимик', score: 14 },
              { id: 2, name: 'Маг', score: 10 },
              { id: 3, name: 'Учёный', score: 8 }
            ])
          }), 1000)
        )
        
        if (!response.ok) throw new Error('Ошибка загрузки')
        users.value = await response.json()
      } catch (err) {
        error.value = err.message
      } finally {
        loading.value = false
      }
    }

    // Загружаем при создании компонента
    fetchLeaderboard()

    return { users, loading, error, fetchLeaderboard }
  }
}
</script>