<template>
  <div class="game-container">
    <div class="unit-details-container">
      <UnitDetails :selectedUnit="selectedUnit" />
    </div>
    <div class="game-board-container">
      <GameBoard :units="units" @selectUnit="selectUnit" @moveUnit="handleMoveUnit" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import GameBoard from '@/components/GameBoard.vue'
import UnitDetails from '@/components/UnitDetails.vue'

const units = ref([
  { row: 2, col: 3, name: 'Unit A', health: 100, attack: 10, id: 0 },
  { row: 4, col: 5, name: 'Unit B', health: 80, attack: 8, id: 1 }
  // Dodaj więcej jednostek według potrzeb
])

const selectedUnit = ref(null)

const selectUnit = (unit) => {
  selectedUnit.value = unit
}
const handleMoveUnit = (unit, newPosition) => {
  // Przekaż jednostkę i nową pozycję do komponentu GameBoard.vue
  // Implementuj odpowiednie logiki związane z przemieszczaniem jednostki
}
</script>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
}

.game-board-container {
  flex: 1;
}

.unit-details-container {
  width: 200px;
  margin-left: 20px;
}
</style>
