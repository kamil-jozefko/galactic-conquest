<template>
  <div
    class="unit"
    v-if="unit"
    :class="{ selected: unit === selectedUnit }"
    @click="selectUnit"
    @dragstart="handleDragStart($event, unit)"
    draggable="true"
  >
    <div class="unit-info">
      <div class="unit-name">{{ unit.name }}</div>
      <div class="unit-stats">
        <div class="stat">
          <span class="stat-label">Health:</span>
          <span class="stat-value">{{ unit.health }}</span>
        </div>
        <div class="stat">
          <span class="stat-label">Attack:</span>
          <span class="stat-value">{{ unit.attack }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps({
  unit: {
    type: Object,
    default: null
  },
  selectedUnit: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['selectUnit', 'moveUnit', 'dragstart'])

const selectUnit = () => {
  emit('selectUnit', props.unit)
}

const handleDragStart = (event) => {
  emit('dragstart', event, props.unit)
}
</script>

<style lang="scss" scoped>
.unit {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 80px;
  height: 80px;
  background-color: #ccc;
  border: 1px solid #aaa;
  cursor: pointer;
  transition: all 0.3s ease;

  &.selected {
    border-color: #f00;
  }

  &.dragging {
    opacity: 0.5;
  }

  &.valid-drop {
    border-color: #00ff00;
  }

  &.invalid-drop {
    border-color: #ff0000;
  }

  .unit-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #000;
    font-size: 14px;

    .unit-name {
      font-weight: bold;
      margin-bottom: 5px;
    }

    .unit-stats {
      display: flex;
      flex-direction: column;

      .stat {
        display: flex;
        align-items: center;
        margin-bottom: 3px;

        .stat-label {
          margin-right: 3px;
          font-weight: bold;
        }

        .stat-value {
          font-weight: normal;
        }
      }
    }
  }
}
</style>
