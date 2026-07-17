# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## New/changed files this round
- `THT_No_Background.png` — updated logo, resized from the original 1024×1024 upload down to 320×320 and compressed (170KB, down from ~1.3MB). It's only ever displayed at ~76px tall, so the original resolution was far more than needed.
- `THT_Banner.png` — new banner image, replaces the text title in the header. Resized to 800px wide (198KB, down from ~1.3MB) for the same reason.
- `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` — regenerated from the updated logo.

If you swap either image again, keep the same filenames, and consider resizing before uploading (roughly 300–800px on the long edge is plenty for how these are actually displayed — full-resolution exports just slow down every page load for no visible benefit).

## Layout
1. Header: logo left, **banner image** (was text) filling the space between logo and hamburger, hamburger sized to fit its own icon.
2. **Hunt Setup / Route Details / Saved Stores / Hot List** — now four tabs in a 2×2 grid just below the header. Tapping one smoothly scrolls you to it as it slides open; tapping again scrolls back up as it closes. Opening any tab also closes an open store card first.
3. Location box, map (day/night toggle, spinning-wheel overlay with a live "Hunting / [store type] / x of y" progress readout and a Cancel button).
4. The selected store's card appears under the map — it now animates open and closed (a quick wipe), and closing one scrolls you back to the top of the page.

## Hunt Setup
- Home address moved out to Route Details, since it's only relevant there.
- A short note under the HUNT! button explains what it does: finds all the stores in your area matching the criteria below it.

## Route Details
- **Home address now lives here**, above Start/End, since that's where it's actually used.

## 📦 Saved Stores (new tab)
Every store you've ever found, across every hunt, in one running list — independent of any single hunt. Check the ones you want and tap **Route from Saved Stores** to build a route from just those (Start/End still come from Route Details, so set those first). It jumps you straight to Route Details showing the result.

## Hot List
- Adding a car now opens its detail card immediately, laid out like a store card (notes on the left, photo on the right).
- Dropped the inline pencil icon — the row itself makes it clear it's tappable.
- No photo yet? The card shows the Treasure Hunt Tracker logo as a placeholder instead of a plain camera icon, still tappable to add a real photo.

## ⭐ Go Pro (hamburger menu)
New button at the top of the menu. For now it just shows a "coming soon" summary of planned Pro features: unlimited Hot List entries, cross-device sign-in, and one-tap import of the current Treasure Hunt / Super Treasure Hunt lists (with images) from hwtreasure.com. Nothing is actually gated yet — this is just the announcement.

## Cleaned-up messaging
Per your note, removed anything that explained *how* the app works internally (timeouts, retry counts, etc.) from user-facing text — that's dev-only detail now, kept solely in this README and in code comments. The footer now also explains that moving data to another device means exporting here and importing there (☰ menu → Data), since there's no server sync.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap (day) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.
- Each Overpass request has a 15-second timeout with automatic mirror fallback, plus a Cancel button on the hunting spinner if something's still slow.

## Reasonable next steps (not built yet)
- Actual Pro tier implementation (today it's an announcement only).
- Cross-device sync (Export/Import covers manual transfer for now).
- True offline support — the manifest is a step toward a full PWA, but there's no service worker yet.
- Smarter route optimization for larger stop counts.

Happy hunting!
