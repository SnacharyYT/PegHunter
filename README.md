# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## New files this round
Add these to the repo root alongside `index.html` (all generated from `THT_No_Background.png`, so if you swap the logo again, regenerate these too and keep the same filenames):
- `manifest.json` — lets Chrome/Android "Install and create shortcut" use a real icon instead of a white-padded fallback.
- `icon-192.png`, `icon-512.png` — referenced by the manifest.
- `apple-touch-icon.png` — used by iOS; has a solid dark background instead of transparency, since iOS renders transparent PNG areas as black on the home screen.

## Layout
1. Header: logo left, title stretched to fill the space between logo and hamburger (taller/narrower via a CSS vertical stretch, per your call), hamburger sized to match the logo's height.
2. **Hunt Setup / Route Details / Hot List tabs now sit right below the header**, not down near the map. Tapping one smoothly scrolls you down to it as it slides open; tapping it again scrolls back up as it closes.
3. Location box, then the map (day/night toggle, spinning-wheel overlay with a live progress count while a hunt runs).
4. The selected store's card appears under the map when you tap a pin or a Route Details entry.
5. The three tab panels themselves still live further down the page — the buttons just moved up top as quick-nav shortcuts.

## Hunt Setup
- "Store Groups" is gone as a separate heading — the chain checkboxes now live directly under **Filters**, alongside a new checkbox list for **Filter by type** (multi-select — check as many as you want, OR logic) and Only show/route favorites (hidden automatically if you have no favorites yet).
- **Car Types to Track** now explains itself: popular types are auto-added per chain but may be wrong — edit a store after your first hunt to correct it.
- **Custom Store** only asks for Name + Address up front; tap "▾ More options" for category, car types, notes, and favorite.
- **Re-running HUNT! after changing your criteria now prunes stale results** — but only auto-found stores that no longer match your checked chains *and* that you haven't touched (no notes, photos, favorite, hunted status, or restock date). Anything you've personalized is never removed by this, even if its chain gets unchecked — only the card's own Remove button deletes those. I built it this way deliberately since blind removal risked deleting real data; flag it if you'd rather it behave differently.

## Route Details
- **Start/End moved back here** from Hunt Setup, and Generate Route now stays disabled until both are actually filled in (defaults like "current location" and "loop back to start" count as filled).
- "Clear board" is now **Show route / Hide route** — a non-destructive toggle for the map line, since the old version implied (correctly) that it might also drop your stores, which it never actually did but was confusingly named.
- The reorder hint now says "after generating a route" to make clear the ▲▼ buttons only appear post-generation.

## Hot List
- Each row now shows a ✏️ pencil to signal it's tappable for details.
- The edit form's Save button is now "💾 Save & Close" instead of a bare Save, and the photo button matches Edit/Remove's styling.

## Store cards
- Notes moved from the left info column to under the photo on the right, balancing the card.
- 🎯 Hunted / 🚫 Skip / 🔥 Fav / ✕ Close all sit in one row next to the name, each with a small label since icons alone aren't self-explanatory on a touch device.

## Data menu (hamburger, bottom section)
- **Export Data** downloads a JSON backup (stores, saved hunts, car types, Hot List).
- **Import Data** loads one back in — on another device, or after clearing this one. Import replaces current data after a confirmation.
- 💣 Clear All Data still wipes everything, with its own confirmation.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap (day) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.

## About the "hunting…" wait time
A truly accurate countdown isn't realistic here — search time depends on network conditions and how busy the free Overpass servers are. Instead, the spinner shows live progress ("Hunting… 3/7") plus a rough estimate (~2s per remaining chain) so you have a sense of how much is left without a number that could be misleadingly precise.

## Reasonable next steps (not built yet)
- **Cross-device sync** — Export/Import now covers manual transfer; a real backend (Firebase/Supabase) would make it automatic.
- **True offline support** — the manifest is a step toward a full PWA, but there's no service worker yet, so it won't work without a connection.
- **Smarter route optimization** — current ordering is a nearest-neighbor heuristic, fine for ~10-20 stops.

Happy hunting!
