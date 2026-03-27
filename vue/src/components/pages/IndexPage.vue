<template>
  Index
  <RouterLink :to="{ name: $routes.ALCHEMY }">
    🧪 Алхимик
  </RouterLink>
  <RouterLink :to="{ name: $routes.API }">
     API
  </RouterLink>
  <div>
    <RouterLink :to="{ name: $routes.EXAMPLE }">
     To Example
    </RouterLink>
  </div>
</template>

<script setup lang="ts">

</script>

<style scoped>

</style>
