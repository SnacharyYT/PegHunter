# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## Assets this depends on
Keep these in the same folder as `index.html` (relative paths, no code changes needed if you swap the art later — just keep the filenames):
- `512_Icon_Dark_SVG.svg` — favicon + header logo
- `wheels-icon-png-31823.png` — spinning wheel shown over the map while a hunt is running
- `THT_512_Icon_Dark.png` — kept in the repo as a PNG fallback/source for future app-icon exports (not currently referenced by the page itself)

## Layout
1. Header: logo on the left, centered title, ☰ menu on the right (Saved Hunts, Clear All Data, Donate, Send a Suggestion, quick guide, app version).
2. Location box — zip/city/address/cross streets, or "📍 Pick on map" (same height as the input now).
3. The map, checkered stripe above and below, day/night toggle in the corner. A spinning wheel overlays the map while a hunt is in progress.
4. The selected store's detail card (appears here when you tap a pin or a Route Details entry).
5. Three tabs — **Hunt Setup**, **Route Details**, **🔥 Hot List** — one open at a time. **Route Details stays disabled the entire time a hunt is running**, not just when there are zero stores, and opens automatically once the hunt finishes.

## The Hunt flow
- Set up your search in **Hunt Setup**, press **🎯 HUNT!**. Starting a new hunt clears out any previously generated route so old and new results can't get mixed up.
- HUNT! greys out once the search finishes and stays that way until you actually change something (location, radius, or checked chains).
- In **Route Details**, Generate Route / Open in Maps / Clear board always sit on one row. Generate greys out after a successful run and re-enables only when you manually reorder a stop or hit Clear board.
- The **"Only show/route favorites"** and car-type filters in Hunt Setup now apply everywhere consistently — the map pins, the Route Order list, and route generation all respect them. (They only affect stores you've already found, not what HUNT! searches for — a brand-new store can't be pre-marked favorite before it's been discovered.)

## Selecting a store
Tap a map pin or a Route Details entry to load that store's card, with a pulsing yellow highlight on the map. The card is a compact 50/50 split: name + 🎯 Hunt / 🚫 Skip / 🔥 Fav / ✕ Close along the top (all labeled, all on one line), category/address/tags/dates/notes on the left, and a large square photo (or a "tap to add" placeholder) on the right. Tap an existing photo for a full-screen view you can page through. Tapping 🎯 Hunt again undoes it. All of this is saved permanently with the store — it's there next time regardless of which hunt you run.

## Store status colors
- **Red** — never hunted · **Yellow** — hunted >7 days ago · **Green** — hunted within 7 days · **Blue** — restock date reached · **Gray** — marked "Skip"

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap (day) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.

Requests are throttled per-service to stay within normal fair-use limits.

## About the version number
The hamburger menu shows "v1.1 — July 2026" as a placeholder — there's no build process tracking this automatically, so bump it by hand in the HTML whenever you want to mark a meaningful update.

## Reasonable next steps (not built yet)
- **Cross-device sync** — needs a small backend (Firebase/Supabase) plus login.
- **True offline PWA support** — a `manifest.json` + service worker for a real home-screen icon and offline use.
- **CSV/JSON export/import** of your store list and Hot List.
- **Smarter route optimization** — current ordering is a nearest-neighbor heuristic, fine for ~10-20 stops.

Happy hunting!
