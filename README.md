# PegHunter 🎯

A store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## What it does
- **🎯 HUNT! button** at the top of the left panel: check whichever store groups/chains you want, then press this once — it searches every checked chain in sequence.
- **Category-level checkboxes:** each group header (Grocery/Big Box, Dollar Stores, etc.) selects or clears every chain in that group in one click.
- **Checking/unchecking a chain never deletes stores you've already found** — checkboxes only control what to search next. Use the Remove button on a store's own card to delete it.
- **Duplicate-location filtering:** results like "Walmart Garden Center" are skipped automatically, and any result within ~120m of a store you already have for that chain is treated as the same physical location.
- **Street name in the store name:** auto-found stores are named like `Dollar Tree: University` instead of a bare chain name repeated for every location.
- **Live map** (Leaflet + OpenStreetMap tiles) showing every saved store as a pin — orange (not yet hunted), teal (hunted), red (avoided) — plus a 🎯 pin for your search center and a 🏠 pin for home.
- **Flexible search center:** zip, city, address, cross streets, or "📍 Pick on map."
- Add custom stores too (gas stations, hardware stores, local toy shops) — address is auto-geocoded.
- **Car type tracking:** default types are Silver Series, Premium, Mainline, and Matchbox; add your own (Johnny Lightning, M2, etc.) in "Car Types to Track." Chains get sensible starting assumptions when first discovered (e.g. Walmart defaults to all four) — edit any individual store any time.
- **One store shown at a time:** click a pin on the map or an item in the Route Order list to view/edit that store's card. Name, address, notes, dates, and car-type tags are locked behind "✏️ Edit"; direct-tap actions are 🎯 Hunted!, 🔥 Favorite, 📷 Photo, Avoid, Edit, Remove.
- **🔥 Favorites:** mark a store, then check "Only show/route favorites" to build a route from just those.
- **Photo gallery:** click a peg photo for a full-screen view with the date on the frame and Prev/Next through that store's history.
- **Two-step routing:** "🧭 Generate Route" draws the route on the map first (numbered stops, real driving line via OSRM) so you can review it; "📲 Open in Google Maps" then hands it off.
- **Drag-to-reorder:** after generating a route, drag stops in the right-side list to rearrange — the map and the Google Maps link follow.
- **Start/End:** current GPS location, Home address, any saved store, or a custom address. End also offers "loop back to start."
- **Saved Hunts:** save your whole setup under a name, reload it later, press HUNT! again.
- **🧹 Clear board** resets route settings only. **💣 Clear All Data** wipes everything, with a confirmation.
- **Mobile:** the Setup and Route Order panels become slide-out drawers (☰ Setup / 📋 Route in the header) so they don't eat screen space, and only one can be open at a time.

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim.
- **Map tiles:** Leaflet.js + OpenStreetMap.
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme — opens the real Google Maps app/site.

Google's free directions link doesn't handle unlimited stops well — the app warns if a route has more than ~9 stops, since very long routes can get truncated.

These OpenStreetMap/OSRM services are free public infrastructure meant for light, hobby-scale use — exactly this app's case. Requests are throttled per-service to stay well within normal fair use. If usage ever scales up a lot (many people hitting it at once), that's the point to look at paid geocoding/routing providers.

## Reasonable next steps (not built yet)
- **Cross-device sync** — would need a small backend (Firebase/Supabase) plus login; right now data lives in one browser only.
- **True offline PWA support** — a `manifest.json` + service worker so it installs as a real app icon and works offline.
- **CSV/JSON export/import** of your store list, for backups or sharing with another hunter.
- **Smarter route optimization** — current ordering is a fast nearest-neighbor heuristic, good for the ~10-20 stop range this is built for; a true shortest-path solve would help on much larger lists.

Happy hunting!
