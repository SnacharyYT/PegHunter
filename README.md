# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## The big one: search reliability

Single-chain hunts working while multi-chain hunts failed "a lot of times" points to the free Overpass service rate-limiting a rapid sequence of broad regex queries from the same client — not a bug in any individual request. Two changes:

- **Automatic retry.** If a chain's search fails, the app now waits 4 seconds and retries once automatically before giving up and reporting it as failed. Most rate-limit blips clear within a few seconds, so this alone should fix a good chunk of the inconsistency.
- **More spacing between requests** (1.2s → 1.5s).

**Being honest about the ceiling here:** this app deliberately uses free, unauthenticated public services (Overpass, Nominatim) with no login and no paid tier, which is what keeps it free to run — but it also means there's a real, external rate limit that no amount of client-side tuning can fully eliminate if you hunt many categories back to back, especially repeatedly in a short session. The retry should absorb most of it. If a specific chain is still consistently failing after a retry, the results summary popup will say so plainly, and trying that one chain by itself (or waiting a minute before hunting again) is the practical workaround.

## New this round

- **7-Eleven** added under a new "Convenience / Gas" group in Hunt Setup.
- **Admin PIN unlock for testing.** Tap the logo in the header, enter `2014`, and every Pro-gated limit (Hot List cap, saved hunts cap, favorites cap) unlocks for testing — no payment, no real account needed. Tap the logo again with the same PIN to lock it back to free-tier behavior. This persists across reloads (stored locally) until you lock it again.
- **Pro feature: Import 2026 Treasure Hunts.** New button in Hot List (visible to everyone, but requires Pro/admin-unlock to actually use) that adds a curated list of confirmed 2026 Super Treasure Hunts as Hot List entries.

### About the 2026 import — please read before relying on it
This is a **hand-typed, one-time snapshot** of 7 confirmed 2026 Super Treasure Hunts, cross-checked against public collector-tracking sites (hwtreasure.com, 164custom.com) as of when this was built. It is **not**:
- An official Mattel list
- Live or auto-updating — new cases get confirmed by collectors throughout the year, and this snapshot won't reflect them until someone manually updates the code
- Complete with photos — I deliberately did not scrape product photos from third-party sites into the app. Car *names* aren't meaningfully copyrightable the way product photography is, but the images on collector sites are, and there's no reliable, permitted way to pull them in client-side anyway (no API, and cross-origin image fetching from arbitrary sites is usually blocked by the browser regardless). Imported cars use the same logo placeholder as any other Hot List entry — tap to add your own photo once you find one.

If you want this to be more current, the list lives in the code as `TH_2026_LIST` — it's a plain array you (or I, in a future round) can update by hand as more cases are confirmed.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim, matching both `name` and `brand` tags.
- **Map tiles:** Leaflet.js + OpenStreetMap (day, default) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.
- Each Overpass request has a 28-second timeout, automatic mirror fallback, one automatic retry on failure, and a results summary after every hunt.

## Reasonable next steps (not built yet)
- A real backend for Pro (accounts, payment, actual gating instead of the PIN test-switch).
- Cross-device sync (Export/Import in the ☰ menu covers manual transfer for now).
- True offline support — the manifest is a step toward a full PWA, but there's no service worker yet.
- A maintained/refreshed TH/STH list, or a real data source for it if one becomes available.

Happy hunting!
