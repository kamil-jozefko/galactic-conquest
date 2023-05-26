<template>
  <div class="game-board">
    <div v-for="row in rows" :key="row" class="board-row">
      <div v-for="col in cols" :key="col" class="board-cell">
        <div class="cell-container" @dragover="handleDragOver" @drop="handleDrop($event, row, col)">
          <GameUnit
            :unit="getUnit(row, col)"
            :selectedUnit="selectedUnit"
            @selectUnit="selectUnit"
            @moveUnit="handleMoveUnit"
            @dragstart="handleDragStart"
          />
        </div>
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

const emit = defineEmits(['selectUnit', 'moveUnit'])

const selectUnit = (unit) => {
  selectedUnit.value = unit
  selectedCell.value = null
}

const handleMoveUnit = (newPosition) => {
  if (selectedUnit.value && isValidMove(selectedUnit.value, newPosition)) {
    selectedUnit.value = { ...selectedUnit.value, ...newPosition }
  }
}

const handleDragOver = (event) => {
  event.preventDefault()
}

const handleDragStart = (event, unit) => {
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('unit', JSON.stringify(unit))
}

const handleDrop = (event, row, col) => {
  event.preventDefault()
  const unitData = event.dataTransfer.getData('unit')
  const unit = JSON.parse(unitData)
  console.log(unit)
  if (unit) {
    moveUnit(unit, { row, col })
  }
}

const moveUnit = (unit, newPosition) => {
  if (isValidMove(unit, newPosition)) {
    const { row, col } = newPosition
    const index = units.value.findIndex((u) => u.id === unit.id);

    console.log(index)
    if (index !== -1) {
      units.value.splice(index, 1) // Usuń jednostkę z obecnej pozycji
      unit.row = row // Zaktualizuj wartość `row` jednostki
      unit.col = col // Zaktualizuj wartość `col` jednostki
      units.value.push(unit) // Dodaj jednostkę na nową pozycję
      selectedUnit.value = unit // Zaktualizuj wybraną jednostkę, jeśli została przesunięta
    }
  }
}

const isValidMove = (unit, newPosition) => {
  const { row, col } = newPosition

  // Sprawdź, czy nowa pozycja jest zajęta przez inną jednostkę
  const isOccupied = units.value.some((u) => u.row === row && u.col === col && u !== unit)
  if (isOccupied) {
    return false
  }

  // Sprawdź, czy nowa pozycja mieści się w zasięgu ruchu jednostki
  const distance = Math.abs(row - unit.row) + Math.abs(col - unit.col)
  if (distance > unit.moveRange) {
    return false
  }

  return true
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
  border: 1px solid #888;
  width: 90px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.cell-container {
  height: 100%;
  width: 100%;
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
