# PegHunter 🎯

A zip-code-based store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting.

## ⚠️ If searches fail: test with a local server, not double-click
Opening this file directly (`file://...`) sometimes causes free map/search APIs to silently reject requests — the "null" origin a `file://` page sends looks different to their servers than a normal website's origin. This is the #1 cause of "search failed" if you've confirmed you're not using the Claude chat preview.

**Fix: serve it locally instead of double-clicking it.** From a terminal in the folder with `index.html`:
```
python3 -m http.server 8000
```
Then open `http://localhost:8000` in Chrome. This takes 10 seconds and resolves the vast majority of these failures. (Node users can do the same with `npx serve`.) The same applies once you deploy for real — GitHub Pages serves over `https://`, so this isn't an issue there at all.

If it still fails after that, open the browser console (F12 → Console tab) and look for the actual error message — the app now logs details there and shows a shorter version in the store row itself.

## What it does right now
- **🎯 HUNT! button** in the header: check whichever store groups/chains you want on the left, then press this once — it searches every checked chain in sequence (throttled and with automatic backup servers, so it won't overload the free services and fail).
- **Category-level checkboxes:** each group header (Grocery/Big Box, Dollar Stores, etc.) has its own checkbox to select or clear every chain in that group in one click.
- **Live map** (Leaflet + OpenStreetMap tiles, free, no key) showing every saved store as a pin — orange (not yet checked), teal (checked), red (avoided) — plus a 🎯 pin for your search center and a 🏠 pin for your home address. Click a pin to jump to that store's card.
- **Flexible search center:** type a zip code, city, address, or cross streets, or click "📍 Pick on map" and tap a point directly on the map.
- Add custom stores too (gas stations, hardware stores, local toy shops, etc.) — address is auto-geocoded if provided.
- Per-store: mark Silver Series / Premium / Matchbox availability, last-checked date, next-restock date (with an overdue flag), notes, and unlimited peg photos over time.
- **Edit mode:** notes and the name/address fields are read-only until you tap "✏️ Edit," so nothing changes by accident.
- **Photo gallery:** click a peg photo to open it full-screen with the visit date shown on the frame, plus Prev/Next through that store's whole photo history.
- **Avoid this store** toggle — skipped automatically on the map and in route building.
- **Two-step route building:** "🧭 Generate Route" computes the order and draws it on the map (numbered stops, actual driving-route line via the free OSRM routing engine) so you can review before committing. "📲 Open in Google Maps" then hands that exact route to Google Maps.
- **Drag-to-reorder:** once a route is generated, drag any stop in the right-side "Route Order" list by its ⠿ handle to manually rearrange it — the map line and the Google Maps link both follow your new order.
- **Start/End options:** current GPS location, your saved Home address, any store on your list, or a custom address. End also offers "loop back to start."
- **Series filter:** show only stores tagged with Silver Series / Premium / Matchbox — others dim out everywhere (map, cards, route list).
- **Saved Hunts** (top header): save your current setup — location, radius, checked chains, avoided chains, series filter — under a name, then Load it back in one click (press HUNT! again afterward to actually run the searches).
- **🧹 Clear board:** resets route start/end/generated route back to default without touching your saved stores.
- **💣 (top header):** wipes every saved store, note, photo, and saved hunt from this device, with a confirmation first.
- All data (including photos) is saved in the browser via `localStorage` — nothing leaves your device except the free map/search/routing lookups described above.
If you've been using an earlier version of this app, stores you added before this update may be missing map coordinates (older versions didn't geocode custom stores). The app now runs a one-time migration on load that re-geocodes any old store with an address but no coordinates — give it a few seconds after opening.

## Why no Google API key is required
This app avoids every paid Google/mapping API by using free alternatives:
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim — free, no key, no billing account. Coverage is excellent in most areas but community-maintained, so occasionally a smaller-town location won't be tagged; that's what the "manual" fallback link and "Add Custom Store" are for.
- **Map display:** Leaflet.js with OpenStreetMap tiles — free, no key.
- **Route preview line on the map:** the free public OSRM demo routing server, which returns an actual driving path — falls back to a straight dashed line if that service is unreachable.
- **Final navigation:** the free, keyless Google Maps Directions URL scheme (`https://www.google.com/maps/dir/?api=1&origin=...&destination=...&waypoints=...`), which just opens Google Maps itself — same navigation experience, zero cost.

One practical limit: Google's free directions link doesn't support unlimited stops well — the app warns you if a route has more than ~9 stops, since very long routes can get truncated and may be worth splitting into two trips.

### Fair-use note on free services
Nominatim, Overpass, and the OSRM demo server are free public services with light rate limits intended for occasional/hobby use — exactly this app's use case. The app now throttles every request to roughly 1 per second and automatically retries Overpass against two backup mirrors if the main server is unavailable, which should keep it well within normal fair-use limits. If you ever expect heavy simultaneous traffic (many people using it at once), look into paid geocoding/routing providers or self-hosting these services at that point.

## Deploying to GitHub Pages
1. Create a new GitHub repo (e.g. `peghunter`)
2. Add this `index.html` to the repo root (README.md is optional but nice to keep)
3. Repo → Settings → Pages → Source: `Deploy from a branch` → Branch: `main` / root
4. Your app will be live at `https://<your-username>.github.io/peghunter/`
5. On a phone, open that URL in Chrome/Safari and use "Add to Home Screen" — it'll behave like an installed app

## Before you launch it publicly
1. **PayPal button:** open `index.html`, find `id="donateBtn"` near the top, and replace `YOUR_PAYPAL_HANDLE` with your real PayPal.me username (or swap in a hosted PayPal donate button if you'd rather).
2. Test the "Find near me" and "Open Route" buttons on both a phone and a desktop browser.

## Reasonable next steps (not built yet, in rough priority order)
- **Cross-device sync:** right now data lives only in one browser. Adding sync means a small backend (e.g. Firebase or Supabase free tier) plus a login
- **True offline PWA support:** add a `manifest.json` + service worker so it installs like a real app icon and caches for offline use
- **CSV/JSON export/import** of your store list and notes, so you can back it up or share it with another hunter
- **Smarter route optimization:** the current ordering is a nearest-neighbor heuristic (fast, no API cost, generally sensible loops) rather than a true shortest-path solve — fine for the ~10-20 stops this app is built for, but a paid Directions API with `optimize:true` would do marginally better on large lists

Happy hunting!
