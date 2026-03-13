<template>
  <div class="api-page">
    <h1>🌐 Пример работы с API</h1>
    <ApiItems />
  </div>
</template>

<script>
import ApiItems from '../../components/pages/ApiItems.vue'

export default {
  name: 'ApiPage',
  components: { ApiItems }
}
</script>

<style scoped>
.api-page {
  padding: 20px;
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}
</style>