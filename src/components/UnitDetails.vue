<template>
  <div class="unit-details">
    <div v-if="selectedUnit">
      <div class="unit-name">{{ selectedUnit.name }}</div>
      <div class="unit-stats">
        <div class="stat">
          <span class="stat-label">Health:</span>
          <span class="stat-value">{{ selectedUnit.health }}</span>
        </div>
        <div class="stat">
          <span class="stat-label">Attack:</span>
          <span class="stat-value">{{ selectedUnit.attack }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const props = defineProps({
  selectedUnit: {
    type: Object,
    default: null
  }
})
</script>

<style scoped>
.unit-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #000;
  font-size: 14px;
  margin-bottom: 20px;
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
