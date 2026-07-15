# PegHunter 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## Layout
Everything is a single vertical stack, designed for one-handed phone use:
1. Centered header, ☰ menu on the right (Saved Hunts, Clear All Data, Donate, and a quick guide).
2. Location box — zip/city/address/cross streets, or "📍 Pick on map."
3. The map, with a 🌙/☀️ day-night toggle in the corner.
4. The selected store's detail card (appears here when you tap a pin or a Route Details entry).
5. Three tabs — **Hunt Setup**, **Route Details**, **🔥 Hot List** — each opens its own panel below; only one is open at a time, and tapping an open tab's button again closes it.

## What's in each tab
- **Hunt Setup:** the 🎯 HUNT! button, search radius, Home address, Route Start/End pickers, car-type/favorites filters, store group checkboxes (auto-discovery via free OpenStreetMap data), Car Types to Track, and the Custom Store form.
- **Route Details:** Generate Route / Open in Google Maps / Clear board, plus the Route Order list — tap a stop to view it, use ▲▼ to reorder (no drag-and-drop, since that doesn't work reliably on touchscreens).
- **🔥 Hot List:** a simple wishlist — add a car you're hunting for, check it off when found, tap it to add a photo and notes.

## Store status colors
Shown as the card's border/pill and the map pin color:
- **Red** — never hunted
- **Yellow** — hunted before, but more than 7 days ago
- **Green** — hunted within the last 7 days
- **Blue** — its restock date has arrived or passed
- **Gray** — marked "Avoid"

## Selecting a store
Tap a map pin or a Route Details entry to load that store's card under the map (with an ✕ to close it). The selected store also gets a slow pulsing yellow glow on the map so it's easy to spot. Name, address, notes, dates, and car-type tags are locked behind "✏️ Edit" — the direct-tap actions are 🎯 Hunted!, 🔥 Favorite, 📷 Photo, Avoid, Edit, Remove.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap (day) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.

Requests are throttled per-service to stay within normal fair-use limits for these free public services.

## Custom app icon
If you design one in Illustrator: keep it a flat vector (no gradients/effects) so it stays legible at 16×16px, export a 512×512 master, then generate 32×32 (favicon), 180×180 (apple-touch-icon), and 192/512 (PWA manifest) sizes from it. For this single-file app, either base64-encode the SVG into the existing `<link rel="icon">` tag, or — since it's on GitHub Pages — add the icon files to the repo, reference them by relative path, and add a small `manifest.json` so "Add to Home Screen" picks up the real icon instead of the emoji placeholder.

## Reasonable next steps (not built yet)
- **Cross-device sync** — needs a small backend (Firebase/Supabase) plus login; data currently lives in one browser only.
- **True offline PWA support** — a `manifest.json` + service worker for a real home-screen icon and offline use (also unlocks the custom-icon integration above).
- **CSV/JSON export/import** of your store list and Hot List, for backups or sharing.
- **Smarter route optimization** — current ordering is a fast nearest-neighbor heuristic; fine for ~10-20 stops, but a true shortest-path solve would help on much larger lists.

Happy hunting!
