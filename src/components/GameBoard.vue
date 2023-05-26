<template>
  <div class="game-board">
    <div class="board-row" v-for="row in rows" :key="row">
      <div
        class="board-cell"
        v-for="col in cols"
        :key="col"
        @click="selectCell(row, col)"
        :class="{ selected: isCellSelected(row, col) }"
      >
        <GameUnit :unit="getUnit(row, col)" :selectedUnit="selectedUnit" @select="selectUnit" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineProps, defineEmits } from 'vue'
import GameUnit from './GameUnit.vue'

const props = defineProps({
  units: {
    type: Array,
    required: true
  }
})

const units = ref(props.units)

const rows = 8
const cols = 8

const selectedUnit = ref(null)
const selectedCell = ref(null)

const emit = defineEmits(['selectUnit'])

const selectUnit = (unit) => {
  if (unit) {
    selectedUnit.value = unit
    selectedCell.value = null
  }
}

const selectCell = (row, col) => {
  if (selectedUnit.value && isCellInRange(row, col)) {
    moveUnit(selectedUnit.value, row, col)
  } else {
    selectedCell.value = { row, col }
  }
}

const moveUnit = (unit, row, col) => {
  unit.row = row
  unit.col = col
  selectedUnit.value = null
  selectedCell.value = null
  emit('selectUnit', unit)
}

const getUnit = (row, col) => {
  return units.value.find((unit) => unit.row === row && unit.col === col)
}

const isCellInRange = (row, col) => {
  const unit = selectedUnit.value
  const distance = Math.abs(row - unit.row) + Math.abs(col - unit.col)
  return distance <= unit.moveRange
}

const isCellSelected = (row, col) => {
  const cell = selectedCell.value
  return cell && cell.row === row && cell.col === col
}

const computedUnits = computed(() => units.value)
</script>

<style scoped>
.game-board {
  display: grid;
  grid-template-rows: repeat(8, 1fr);
  gap: 4px;
  padding: 20px;
  max-width: 640px; /* Ograniczenie szerokości planszy */
  margin: 0 auto; /* Wycentrowanie planszy */
}

.board-row {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
}

.board-cell {
  background-color: #ddd;
  padding: 10px;
  cursor: pointer;
  border: 1px solid #888; /* Dodane obramowanie */
}

.board-cell.selected {
  background-color: #aaf;
  border: 2px solid #00f;
}

@media (max-width: 600px) {
  .game-board {
    padding: 10px;
  }

  .board-cell {
    padding: 6px;
  }
}
</style>
