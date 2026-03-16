<template>
  <div class="map-legend">
    <div class="legend-title">Operator</div>
    <div v-for="(color, operator) in operatorColors" :key="operator" class="legend-item">
      <span class="legend-dot" :style="{ background: color }" />
      <span class="legend-label">{{ operatorLabels[operator] }}</span>
    </div>
  </div>
</template>

<script setup>
import { operatorColors, operatorLabels } from '../constants.js'
</script>

<style scoped>
.map-legend {
  position: absolute;
  bottom: 30px;
  right: 10px;
  z-index: 1000;
  background: rgba(255, 255, 255, 0.92);
  border: 1px solid #ccc;
  border-radius: 6px;
  padding: 10px 14px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  pointer-events: none;
}

.legend-title {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #555;
  margin-bottom: 7px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 5px;
}

.legend-item:last-child {
  margin-bottom: 0;
}

.legend-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  flex-shrink: 0;
  border: 1px solid rgba(0,0,0,0.15);
}

.legend-label {
  font-size: 12px;
  color: #333;
}
</style>
