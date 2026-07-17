# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## This round: three real bugs found and fixed

**"None found" / "search failed" on multi-chain hunts — actual cause found.** When a chain's search came back with results that were *all* stores you'd already found in an earlier hunt, the code reported "none found nearby" — which reads exactly like a failure, even though the search worked correctly. Now it distinguishes three outcomes and says so plainly: `✅ 3 found`, `🔁 no new stores (2 already in your list)`, or `⚪ none found nearby`. Also spaced out the requests between chains a bit more (600ms → 1200ms) to reduce the chance of the free Overpass server rate-limiting a long run of searches, which could plausibly cause real (not just mislabeled) failures on multi-chain hunts.

**Discovered stores no longer disappear when you change hunt criteria.** Removed the "smart pruning" behavior entirely — a previous-round decision to auto-remove untouched stores whose chain got unchecked. Turns out that wasn't wanted. Saved Stores is now a strictly append-only list: everything ever found stays there permanently until you explicitly hit Remove on that store's card. Re-hunting with different criteria only ever *adds*, never removes.

**Generate Route now re-enables when you Skip or Remove a store**, not just when Start/End changes. Previously, skipping or removing a store from its card left Generate Route greyed out with no way to recompute — now either action immediately makes it clickable again.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim, matching both `name` and `brand` tags.
- **Map tiles:** Leaflet.js + OpenStreetMap (day, default) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.
- Each Overpass request has a 28-second timeout with automatic mirror fallback, requests are now spaced 1.2s apart, and a results summary shows exactly what happened per chain after every hunt.

## Reasonable next steps (not built yet)
- Actual Pro tier implementation (accounts, payment, real gating — today the free-tier caps are enforced but nothing is behind a real login).
- Cross-device sync (Export/Import in the ☰ menu covers manual transfer for now).
- True offline support — the manifest is a step toward a full PWA, but there's no service worker yet.
- hwtreasure.com integration for auto-loading TH/STH lists.

Happy hunting!
