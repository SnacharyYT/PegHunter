# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## This round's changes

**All popups are now in-app.** Every `alert()`/`confirm()`/`prompt()` (browser-native dialogs) has been replaced with a themed in-app modal — notices, confirmations, and the "name this hunt" prompt all now match the app's look instead of looking like a browser system dialog.

**Free-tier limits + Pro upsell.** Non-Pro accounts are capped at 5 Hot List cars, 3 saved hunts, and 5 favorite stores. Hitting a cap shows the Pro modal instead of silently failing. There's no real Pro tier yet — `isPro` is hardcoded `false` — this is groundwork/announcement only. When you're ready to build real Pro gating (accounts, payments), search for `isPro` and `FREE_LIMITS` in the script.

**Hot List now rolls out from the row you tapped**, not from a shared area at the bottom — tap a car and its card unfurls right underneath it; tap the ✕ and it rolls back up, then the page scrolls to the top.

**Photos are now 2:3 (portrait)** instead of square, on both store cards and Hot List cards — closer to an actual Hot Wheels blister card's proportions.

**Map defaults to day mode** on load — night mode was hard to read at a glance; the toggle is still there if you prefer it.

**Broader store search:** each hunt now also matches OpenStreetMap's `brand` tag, not just `name` — some real-world locations are tagged with a brand instead of (or in addition to) a name, so this should catch stores that were being missed. If you're still seeing a real, nearby location get missed after this, it likely just isn't mapped in OpenStreetMap yet — that's a data-completeness limit outside the app's control, not a bug in the search itself.

**Home address moved to Route Details** (was in Hunt Setup), since it's only used there.

## Layout
1. Header: logo, **banner image** (replaces the old text title), hamburger.
2. **Hunt Setup / Route Details / Saved Stores / Hot List** — four tabs in a 2×2 grid below the header, each opening as a smooth slide-down panel.
3. Location box, map (day/night toggle — defaults to day — plus a spinning-wheel overlay with a live "Hunting / [store type] / x of y" readout and a Cancel button).
4. The selected store's card appears under the map, animating open/closed, scrolling to the top when closed.

## 📦 Saved Stores
Every store you've ever found, across every hunt, in one running list. Check the ones you want and tap **Route from Saved Stores** to build a route from just those (Start/End come from Route Details, so set those first).

## ⭐ Go Pro (hamburger menu)
Shows a "coming soon" summary: unlimited Hot List/saved hunts/favorites, cross-device sign-in, and one-tap import of the current Treasure Hunt / Super Treasure Hunt lists (with images) from hwtreasure.com. Nothing is gated behind a real account yet.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim (now matching on both `name` and `brand` tags).
- **Map tiles:** Leaflet.js + OpenStreetMap (day, default) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.
- Each Overpass request has a 15-second timeout with automatic mirror fallback, plus a Cancel button on the hunting spinner.

## Reasonable next steps (not built yet)
- Actual Pro tier implementation (accounts, payment, real gating — today it's a UI announcement plus soft caps only).
- Cross-device sync (Export/Import in the ☰ menu covers manual transfer for now).
- True offline support — the manifest is a step toward a full PWA, but there's no service worker yet.
- hwtreasure.com integration for auto-loading TH/STH lists.

Happy hunting!
