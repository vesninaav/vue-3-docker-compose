<template>
  <div class="alchemy">
    <ElementsField />
    <CraftTable />
    <Leaderboard/>
  </div>
</template>

<script>
import ElementsField from '../alchemy/ElementsField.vue'
import CraftTable from '../alchemy/CraftTable.vue'
import Leaderboard from '../alchemy/Leaderboard.vue'
import { watch } from 'vue'
import { useStore } from 'vuex'

export default {
  name: 'AlchemyPage',
  components: { ElementsField, CraftTable , Leaderboard},
    setup() {
    const store = useStore()
    
    watch(() => store.state.discovered, (newVal, oldVal) => {
      if (newVal.length > oldVal.length) {
        console.log('🎉 Открыт новый элемент!', newVal[newVal.length - 1])
      }
    }, { deep: true })
  }
}
</script>

<style scoped>
.alchemy {
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #f0f4f8, #c3cfe2);
  box-sizing: border-box;
  overflow: hidden;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
</style>