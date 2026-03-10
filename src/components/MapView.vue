<template>
  <div class="map-wrapper">
    <div ref="mapContainer" class="map-container" />
    <MapLegend />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import L from 'leaflet'
import events from '../data/events.json'
import { categoryColors, tileLayers } from '../constants.js'
import MapLegend from './MapLegend.vue'

const props = defineProps({
  activeEra: {
    type: String,
    required: true
  },
  mapLang: {
    type: String,
    required: true
  }
})

const mapContainer = ref(null)
const markers = ref([])
const activeTileLayer = ref(null)
let map = null

function makeIcon(category) {
  const color = categoryColors[category] || '#888'
  const svg = `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 36" width="24" height="36">
    <path d="M12 0C5.373 0 0 5.373 0 12c0 9 12 24 12 24s12-15 12-24C24 5.373 18.627 0 12 0z"
      fill="${color}" stroke="#fff" stroke-width="1.5"/>
    <circle cx="12" cy="12" r="5" fill="#fff" opacity="0.9"/>
  </svg>`
  return L.divIcon({
    html: svg,
    className: '',
    iconSize: [24, 36],
    iconAnchor: [12, 36],
    popupAnchor: [0, -36]
  })
}

function buildPopup(event) {
  const siteHtml = event.contemporarySite
    ? `<div class="event-site">
        <strong>Visit today:</strong> ${event.contemporarySite.name}
        <br>
        <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(event.contemporarySite.query)}"
           target="_blank" rel="noopener noreferrer" class="maps-link">View on Google Maps →</a>
      </div>`
    : ''

  return `
    <div class="event-popup">
      <h3>${event.name}</h3>
      <div class="event-meta">
        <span class="event-year">${event.year}</span>
        <span class="event-period">${event.period} Period</span>
        <span class="event-category">${event.category}</span>
      </div>
      <p class="event-description">${event.description}</p>
      ${siteHtml}
    </div>
  `
}

function updateMarkers(era) {
  markers.value.forEach(({ marker, event }) => {
    if (era === 'all' || event.era === era) {
      marker.addTo(map)
    } else {
      marker.remove()
    }
  })
}

function swapTileLayer(lang) {
  if (activeTileLayer.value) {
    activeTileLayer.value.remove()
  }
  const { url, attribution } = tileLayers[lang]
  activeTileLayer.value = L.tileLayer(url, { attribution, maxZoom: 18 }).addTo(map)
}

onMounted(() => {
  map = L.map(mapContainer.value).setView([36.5, 137.5], 6)

  const { url, attribution } = tileLayers[props.mapLang]
  activeTileLayer.value = L.tileLayer(url, { attribution, maxZoom: 18 }).addTo(map)

  markers.value = events.map(event => {
    const marker = L.marker([event.lat, event.lng], { icon: makeIcon(event.category) })
      .bindPopup(buildPopup(event), { maxWidth: 340 })
    marker.addTo(map)
    return { marker, event }
  })
})

watch(() => props.activeEra, updateMarkers)
watch(() => props.mapLang, swapTileLayer)
</script>

<style>
.map-wrapper {
  flex: 1;
  position: relative;
  width: 100%;
}

.map-container {
  width: 100%;
  height: 100%;
}

/* Popup styles (not scoped — Leaflet renders popups outside the component) */
.event-popup {
  font-family: system-ui, sans-serif;
  min-width: 240px;
}

.event-popup h3 {
  margin: 0 0 6px;
  font-size: 15px;
  font-weight: 700;
  color: #1a1a2e;
  line-height: 1.3;
}

.event-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 8px;
}

.event-meta span {
  font-size: 11px;
  padding: 2px 7px;
  border-radius: 3px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.4px;
}

.event-year {
  background: #1a1a2e;
  color: #fff;
}

.event-period {
  background: #eee;
  color: #444;
}

.event-category {
  background: #f0e6d3;
  color: #7a4a1e;
}

.event-description {
  margin: 0 0 10px;
  font-size: 13px;
  color: #333;
  line-height: 1.5;
}

.event-site {
  font-size: 12px;
  color: #444;
  border-top: 1px solid #e8e8e8;
  padding-top: 8px;
  line-height: 1.6;
}

.maps-link {
  color: #2980b9;
  text-decoration: none;
  font-weight: 600;
}

.maps-link:hover {
  text-decoration: underline;
}
</style>
