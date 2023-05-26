<template>
  <div class="unit" v-if="unit" :class="{ selected: unit === selectedUnit }" @click="selectUnit">
    <div class="unit-info">
      <div class="unit-name">{{ unit?.name }}</div>
      <div class="unit-stats">
        <div class="stat">
          <span class="stat-label">Health:</span>
          <span class="stat-value">{{ unit?.health }}</span>
        </div>
        <div class="stat">
          <span class="stat-label">Attack:</span>
          <span class="stat-value">{{ unit?.attack }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
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

const selectUnit = () => {
  emit('select', props.unit)
}
const emit = defineEmits(['select'])
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
}

.unit.selected {
  border-color: #f00;
}

.unit-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #000;
  font-size: 14px;
}

.unit-name {
  font-weight: bold;
  margin-bottom: 5px;
}

.unit-stats {
  display: flex;
  flex-direction: column;
}

.stat {
  display: flex;
  align-items: center;
  margin-bottom: 3px;
}

.stat-label {
  margin-right: 3px;
  font-weight: bold;
}

.stat-value {
  font-weight: normal;
}
</style>
