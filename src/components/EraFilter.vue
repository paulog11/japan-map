<template>
  <div class="era-filter">
    <span class="era-label">Filter by era:</span>
    <button
      v-for="option in options"
      :key="option.value"
      :class="['era-btn', { active: activeEra === option.value }]"
      @click="$emit('update:activeEra', option.value)"
    >
      {{ option.label }}
    </button>
  </div>
</template>

<script setup>
defineProps({
  activeEra: {
    type: String,
    required: true
  }
})

defineEmits(['update:activeEra'])

const options = [
  { value: 'all', label: 'All' },
  { value: 'medieval', label: 'Medieval (1185–1615)' },
  { value: 'edo', label: 'Edo (1603–1868)' }
]
</script>

<style scoped>
.era-filter {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  background: #1a1a2e;
  border-bottom: 1px solid #2a2a4e;
}

.era-label {
  color: #aaa;
  font-size: 13px;
  margin-right: 4px;
  white-space: nowrap;
}

.era-btn {
  padding: 6px 14px;
  border: 1px solid #444;
  border-radius: 4px;
  background: transparent;
  color: #ccc;
  font-size: 13px;
  cursor: pointer;
  transition: background 0.15s, color 0.15s, border-color 0.15s;
}

.era-btn:hover {
  background: #2a2a4e;
  border-color: #666;
  color: #fff;
}

.era-btn.active {
  background: #c0392b;
  border-color: #c0392b;
  color: #fff;
  font-weight: 600;
}
</style>
