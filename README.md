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
- **🎯 HUNT! button** at the top of the left panel: check whichever store groups/chains you want, then press this once — it searches every checked chain in sequence (throttled, with automatic backup servers).
- **Category-level checkboxes:** each group header (Grocery/Big Box, Dollar Stores, etc.) has its own checkbox to select or clear every chain in that group in one click.
- **Checking/unchecking a chain never deletes stores you've already found.** Checkboxes are purely "what to search for next time you hit Hunt!" To remove a store entirely, use the Remove button on its card.
- **Duplicate-location filtering:** results named things like "Walmart Garden Center" are skipped automatically, and any result within about 120m of a store you already have for that chain is treated as the same physical location and skipped — this stops big-box sub-departments from doubling up your list.
- **Street name in the store name:** auto-found stores are named like `Dollar Tree: University` (chain + short street name) instead of a bare chain name repeated for every location.
- **Live map** (Leaflet + OpenStreetMap tiles, free, no key) showing every saved store as a pin — orange (not yet checked), teal (checked), red (avoided) — plus a 🎯 pin for your search center and a 🏠 pin for your home address.
- **Flexible search center:** type a zip code, city, address, or cross streets, or click "📍 Pick on map."
- Add custom stores too (gas stations, hardware stores, local toy shops, etc.) — address is auto-geocoded if provided.
- **Car type tracking:** default types are Silver Series, Premium, Mainline, and Matchbox, and you can add your own custom types (Johnny Lightning, M2 Machines, whatever you collect) in the left panel's "Car Types to Track" section. These become checkboxes on every store's Edit form and options in the "Filter route/display by car type" dropdown.
- **Locked-down editing:** name, address, notes, dates, and car-type tags are read-only until you tap "✏️ Edit" — nothing changes by an accidental click. The card's direct-action buttons are just: 🎯 Hunted! (stamps today's date), ⭐ Favorite, 📷 Peg photo, Avoid, Edit, Remove.
- **⭐ Favorites:** star any store, then check "Only show/route favorites" in the left panel to build a route from just your favorites.
- **Photo gallery:** click a peg photo to open it full-screen with the visit date on the frame, plus Prev/Next through that store's whole photo history.
- **Two-step route building:** "🧭 Generate Route" draws the route on the map (numbered stops, real driving line via the free OSRM routing engine) before you commit. "📲 Open in Google Maps" then hands that exact route to Google Maps.
- **Drag-to-reorder:** once a route is generated, drag any stop in the right-side "Route Order" list by its ⠿ handle — the map line and the Google Maps link both follow your new order.
- **Start/End options:** current GPS location, your saved Home address, any store on your list, or a custom address. End also offers "loop back to start."
- **Saved Hunts** (top header): save your current setup — location, radius, checked chains, series filter, favorites-only — under a name, then Load it back and press HUNT! again.
- **🧹 Clear board:** resets route start/end/generated route back to default without touching your saved stores.
- **💣 Clear All Data** (top header, labeled): wipes every saved store, note, photo, and saved hunt from this device, with a confirmation first.
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
