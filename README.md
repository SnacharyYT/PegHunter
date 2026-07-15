# Treasure Hunt Tracker 🎯

(Formerly "PegHunter" — renamed to avoid an unfortunate abbreviation.)

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## Layout
Everything is a single vertical stack, designed for one-handed phone use:
1. Centered header with your custom logo, ☰ menu on the right (Saved Hunts, Clear All Data, Donate, quick guide).
2. Location box — zip/city/address/cross streets, or "📍 Pick on map."
3. The map, with a day/night toggle in the corner, and a checkered stripe above and below it.
4. The selected store's detail card (appears here when you tap a pin or a Route Details entry).
5. Three tabs — **Hunt Setup**, **Route Details**, **🔥 Hot List** — one open at a time; tapping an open tab's button again closes it. **Route Details is disabled/greyed out until you've found at least one store.**

## The Hunt flow
- Set up your search in **Hunt Setup** (location, radius, store groups, Start/End, filters), then press **🎯 HUNT!**.
- Once the search finishes, the HUNT! button greys out and **Route Details opens automatically**. HUNT! stays disabled until you actually change something (location, radius, or which chains are checked) — it's not meant to be clickable again with nothing new to search for.
- In **Route Details**: Generate Route / Open in Maps / Clear board always sit on one row (no wrapping, no icons — just enough room for the labels). After a successful Generate, that button greys out too, since regenerating with no changes would just recompute the same order — it re-enables the moment you manually reorder a stop with the ▲▼ buttons, or hit Clear board.
- The Route Order list updates the map instantly when you reorder stops.

## Selecting a store
Tap a map pin or a Route Details entry to load that store's card under the map, with a pulsing yellow highlight marking it on the map and an ✕ to close the card. All custom info you add to a store (notes, photos, tags, hunted/skip/favorite status) is saved with that store permanently — it's there the next time you hunt, whether or not that chain gets searched again.

- Store name and three quick-tap icon buttons (🎯 Hunted, 🚫 Skip, 🔥 Favorite) share one row and never wrap. Tapping 🎯 Hunted again undoes it, in case of a misclick.
- Left column: status, tags, dates, notes. Right column: your peg photo, or a "tap to add" placeholder if you haven't uploaded one yet. Tap an existing photo for a full-screen view you can page through.
- Name, address, notes, dates, and car-type tags stay locked behind "✏️ Edit" so nothing changes by accident.

## Store status colors
Shown as the card's border/pill and the map pin color:
- **Red** — never hunted
- **Yellow** — hunted before, but more than 7 days ago
- **Green** — hunted within the last 7 days
- **Blue** — its restock date has arrived or passed
- **Gray** — marked "Skip"

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap (day) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.

Requests are throttled per-service to stay within normal fair-use limits for these free public services.

## Custom icon
`THT_512_Icon.svg` lives alongside `index.html` in the repo and is used as both the favicon and the header logo. If you ever swap the art, just replace that file with the same name — no code changes needed. For a future PWA "Add to Home Screen" icon, you'd want a `manifest.json` plus PNG exports at 192×192 and 512×512 from the same master.

## Reasonable next steps (not built yet)
- **Cross-device sync** — needs a small backend (Firebase/Supabase) plus login; data currently lives in one browser only.
- **True offline PWA support** — a `manifest.json` + service worker for a real home-screen icon and offline use.
- **CSV/JSON export/import** of your store list and Hot List, for backups or sharing.
- **Smarter route optimization** — current ordering is a fast nearest-neighbor heuristic; fine for ~10-20 stops, but a true shortest-path solve would help on much larger lists.

Happy hunting!
