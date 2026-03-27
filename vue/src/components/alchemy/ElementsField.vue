<template>
  <div class="field-container">
    <!-- Добавляем поиск -->
    <div class="search-container">
      <input 
        v-model="searchQuery" 
        type="text" 
        placeholder="Поиск элементов..."
        class="search-input"
      >
      <span v-if="searchQuery" class="search-clear" @click="searchQuery = ''">✕</span>
      <span class="search-count" v-if="searchQuery">
        Найдено: {{ filteredElements.length }}
      </span>
    </div>

    <!-- Использование props из ElementCard -->
    <div class="field">
      <ElementCard
        v-for="id in filteredElements"
        :key="id"
        :id="id"
        @select="() => add(id)"
      />
    </div>

    <!-- Сообщение если ничего не найдено -->
    <div v-if="searchQuery && filteredElements.length === 0" class="no-results">
      ⚠️ Элементы не найдены
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import ElementCard from './ElementCard.vue'
import { ELEMENTS } from '../../config/elements'

export default {
  components: { ElementCard },

  data() {
    return {
      searchQuery: ''
    }
  },
//Computed свойства
  computed: {
    ...mapGetters(['discoveredElements']),
    
    elements() {
      return this.discoveredElements
    },

    // Фильтруем элементы по поисковому запросу
    filteredElements() {
      if (!this.searchQuery.trim()) {
        return this.elements
      }

      const query = this.searchQuery.toLowerCase().trim()
      
      return this.elements.filter(id => {
        const element = ELEMENTS[id]
        return element.name.toLowerCase().includes(query) ||
               element.icon.includes(query)
      })
    }
  },

  methods: {
    ...mapActions(['addToTable']),

    add(id) {
      this.addToTable(id)
    }
  }
}
</script>

<style scoped lang="scss">
.field-container {
  flex: 8;
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}

.search-container {
  position: relative;
  padding: 10px 20px 5px 20px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(5px);
  z-index: 10;
}

.search-input {
  width: 100%;
  padding: 12px 40px 12px 16px;
  border: 2px solid #e0e0e0;
  border-radius: 30px;
  font-size: 16px;
  transition: all 0.3s ease;
  background: white;
  box-sizing: border-box;

  &:focus {
    outline: none;
    border-color: #9b59b6;
    box-shadow: 0 0 0 3px rgba(155, 89, 182, 0.1);
  }

  &::placeholder {
    color: #999;
    font-style: italic;
  }
}

.search-clear {
  position: absolute;
  right: 30px;
  top: 50%;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background: #e0e0e0;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s;

  &:hover {
    background: #ff6b6b;
    color: white;
  }
}

.search-count {
  position: absolute;
  right: 60px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 14px;
  color: #666;
  background: #f0f0f0;
  padding: 4px 10px;
  border-radius: 20px;
}

.field {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  gap: 15px;
  padding: 20px;
  overflow-y: auto;
  box-sizing: border-box;
  width: 100%;
  max-width: 100%;
  align-content: flex-start;
  flex: 1;

  &::-webkit-scrollbar {
    width: 8px;
  }

  &::-webkit-scrollbar-track {
    background: rgba(0,0,0,0.05);
    border-radius: 4px;
  }

  &::-webkit-scrollbar-thumb {
    background: rgba(0,0,0,0.2);
    border-radius: 4px;

    &:hover {
      background: rgba(0,0,0,0.3);
    }
  }
}

.no-results {
  text-align: center;
  padding: 40px;
  color: #666;
  font-size: 18px;
  background: rgba(255, 255, 255, 0.5);
  margin: 20px;
  border-radius: 20px;
}
</style>