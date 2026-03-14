# Japan History Map

Interactive map app that displays significant events in Japanese history as markers on a Leaflet map.

## Tech Stack

- **Vue 3** (Composition API, `<script setup>`) + **Vite**
- **Leaflet** for the map (no vue-leaflet wrapper — raw Leaflet in a Vue component)
- No router, no state management library — simple prop/emit flow

## Project Structure

```
src/
  App.vue              — root layout: header, era filter, map
  main.js              — app entry point
  constants.js         — shared categoryColors, categoryLabels, tileLayers
  style.css            — global styles + Leaflet CSS import
  components/
    MapView.vue        — Leaflet map, markers, popups, tile layer swapping
    MapLegend.vue      — color legend overlay (bottom-right of map)
    EraFilter.vue      — era toggle buttons (top bar)
  data/
    events.json        — all historical event data (static JSON, no API)
```

## Key Conventions

- **Event data** lives in `src/data/events.json`. Each event has: `id`, `name`, `year`, `era`, `period`, `category`, `description`, `lat`, `lng`, `contemporarySite` (nullable object with `name` and `query` for Google Maps link).
- **Categories**: `battle`, `political`, `cultural`, `disaster` — colors defined in `constants.js`.
- **Eras**: `ancient` (pre-710), `classical` (710–1185), `medieval` (1185–1615), `edo` (1603–1868), `modern` (1868–1945), `postwar` (1945–present) — used by the era filter. The filter value `"all"` shows everything. 60 events total (10 per era).
- **Map language toggle**: swaps tile layer between OSM Japan (Japanese labels) and CartoDB Voyager (English/romanized labels). Controlled via `mapLang` prop (`"ja"` | `"en"`).
- Popup HTML is built as raw strings in `buildPopup()` inside MapView — Leaflet renders outside Vue's virtual DOM.

## Commands

- `npm run dev` — start dev server
- `npm run build` — production build to `dist/`
- `npm run preview` — preview production build
