<template>
  <div class="app">
    <header class="app-header">
      <h1>Japan History Map</h1>
      <button class="lang-toggle" @click="toggleLang">
        {{ mapLang === 'ja' ? 'EN' : '日本語' }}
      </button>
    </header>
    <EraFilter v-model:activeEra="activeEra" />
    <MapView :activeEra="activeEra" :mapLang="mapLang" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import EraFilter from './components/EraFilter.vue'
import MapView from './components/MapView.vue'

const activeEra = ref('all')
const mapLang = ref('ja')

function toggleLang() {
  mapLang.value = mapLang.value === 'ja' ? 'en' : 'ja'
}
</script>

<style>
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  background: #0d0d1a;
}

.app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: #1a1a2e;
  padding: 10px 16px;
  border-bottom: 1px solid #2a2a4e;
}

.app-header h1 {
  color: #fff;
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.lang-toggle {
  padding: 5px 14px;
  border: 1px solid #555;
  border-radius: 4px;
  background: transparent;
  color: #ccc;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.15s, color 0.15s, border-color 0.15s;
}

.lang-toggle:hover {
  background: #2a2a4e;
  border-color: #888;
  color: #fff;
}
</style>
