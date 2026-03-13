<template>
  <div class="api-items">
    <h3>👟 Магазин кроссовок (пример API)</h3>
    
    <!-- Кнопка для загрузки -->
    <button 
      @click="fetchItems" 
      :disabled="loading"
      class="fetch-button"
    >
      <span v-if="loading">Загрузка...</span>
      <span v-else>🔄 Загрузить товары</span>
    </button>

    <!-- Состояние загрузки -->
    <div v-if="loading" class="status loading">
      ⏳ Получение данных с сервера...
    </div>

    <!-- Сообщение об ошибке -->
    <div v-if="error" class="status error">
      ❌ Ошибка: {{ error }}
      <button @click="error = null" class="dismiss-btn">✕</button>
    </div>

    <!-- Сетка с товарами -->
    <div v-if="items.length > 0" class="items-grid">
      <div 
        v-for="item in filteredItems" 
        :key="item.id"
        class="item-card"
      >
        <img 
          :src="`https://61ecf1a9fdc45e18.mokky.dev${item.imageUrl}`" 
          :alt="item.title"
          class="item-image"
          @error="handleImageError"
        >
        <div class="item-info">
          <h4>{{ item.title }}</h4>
          <p class="description">{{ item.description }}</p>
          <p class="price">{{ formatPrice(item.price) }}</p>
        </div>
      </div>
    </div>

    <!-- Пустое состояние -->
    <div v-else-if="!loading && !error" class="empty-state">
      <p>📦 Нажмите кнопку, чтобы загрузить товары</p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  name: 'ApiItems',
  
  setup() {
    // Реактивные состояния
    const items = ref([])
    const loading = ref(false)
    const error = ref(null)

    // Форматирование цены
    const formatPrice = (price) => {
      return new Intl.NumberFormat('ru-RU', {
        style: 'currency',
        currency: 'RUB',
        minimumFractionDigits: 0
      }).format(price)
    }

    // Фильтрация только товаров (исключаем записи с username)
    const filteredItems = computed(() => {
      return items.value.filter(item => item.title && item.price)
    })

    // Обработка ошибок загрузки изображений
    const handleImageError = (e) => {
      e.target.src = 'https://via.placeholder.com/200x150?text=Нет+фото'
    }

    // Функция для запроса к API
    const fetchItems = async () => {
      loading.value = true
      error.value = null
      
      try {
        const response = await fetch('https://61ecf1a9fdc45e18.mokky.dev/items')
        
        if (!response.ok) {
          throw new Error(`HTTP ошибка ${response.status}`)
        }
        
        const data = await response.json()
        console.log('Получены данные:', data)
        
        items.value = data
        
      } catch (err) {
        console.error('Ошибка запроса:', err)
        error.value = err.message
      } finally {
        loading.value = false
      }
    }

    return {
      items,
      loading,
      error,
      filteredItems,
      formatPrice,
      handleImageError,
      fetchItems
    }
  }
}
</script>

<style scoped lang="scss">
.api-items {
  padding: 20px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  max-width: 100%;
  overflow: hidden;

  h3 {
    margin-top: 0;
    margin-bottom: 20px;
    color: #333;
    font-size: 1.5rem;
  }
}

.fetch-button {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 30px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-bottom: 20px;
  width: 100%;
  max-width: 300px;

  &:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(102, 126, 234, 0.4);
  }

  &:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }
}

.status {
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;

  &.loading {
    background: #e3f2fd;
    color: #1976d2;
  }

  &.error {
    background: #ffebee;
    color: #c62828;
  }
}

.dismiss-btn {
  background: none;
  border: none;
  color: #666;
  cursor: pointer;
  font-size: 18px;
  padding: 0 5px;

  &:hover {
    color: #333;
  }
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  max-height: 500px;
  overflow-y: auto;
  padding: 5px;
}

.item-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
  }
}

.item-image {
  width: 100%;
  height: 180px;
  object-fit: cover;
  border-bottom: 2px solid #f0f0f0;
}

.item-info {
  padding: 15px;

  h4 {
    margin: 0 0 10px 0;
    color: #333;
    font-size: 1.1rem;
  }

  .description {
    color: #666;
    font-size: 0.9rem;
    margin: 0 0 10px 0;
    line-height: 1.4;
  }

  .price {
    color: #2ecc71;
    font-weight: bold;
    font-size: 1.2rem;
    margin: 0;
  }
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: #999;
  font-size: 1.1rem;
  background: #f9f9f9;
  border-radius: 10px;
  margin-top: 20px;
}
</style>