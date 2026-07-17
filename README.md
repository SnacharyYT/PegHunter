# Treasure Hunt Tracker 🎯

A mobile-first store tracker and route planner for Hot Wheels (and Matchbox / diecast) hunting. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## This round: bug fixes

**Missing stores — likely root cause found and fixed.** The 15-second client-side search timeout added a couple rounds back (to stop hunts from hanging forever) was almost certainly too aggressive — especially after search queries got more thorough (matching both `name` and `brand` tags), which made each search take longer. A slow-but-legitimate search would get cut off and silently return nothing. Raised the timeout to 28 seconds (well above the server's own 25-second budget) so real results have time to come back. **There's also a new post-hunt summary popup** listing exactly what happened per chain — found (with count), none nearby, or failed — so if something is still coming up empty, you'll see it immediately instead of it being buried in a screen you've already navigated away from.

**Generate Route now correctly re-enables** when you change Start or End after a route was already generated. Previously it stayed greyed out.

**The map highlight was getting hidden behind route markers.** Found the actual cause: Leaflet renders regular markers and the highlight circle in different stacking layers by default, and the marker layer sits on top — so whenever a route was displayed, its numbered stop markers always covered the yellow highlight underneath. Fixed by giving the highlight its own layer explicitly placed above everything else.

**Location and Home Address now have explicit Save buttons** with a confirmation toast ("✓ Location set: ...") so it's clear the input was actually captured — including a clear message if the address couldn't be found at all.

**Route Details list clicks now scroll to the map** (to see the highlighted dot) instead of straight to the card. Saved Stores clicks still scroll to the card, since you said that pairing was working well.

**Hot List:**
- Opening a card now scrolls it to the center of the screen.
- Closing a card no longer jumps you back to the top of the page (only store cards still do that, since that behavior wasn't reported as unwanted there).

## Free services used (no API keys, no billing)
- **Store discovery & geocoding:** OpenStreetMap's Overpass API and Nominatim, matching both `name` and `brand` tags.
- **Map tiles:** Leaflet.js + OpenStreetMap (day, default) / CARTO Dark Matter (night).
- **Route preview line:** the public OSRM demo routing server (falls back to a straight dashed line if unreachable).
- **Turn-by-turn navigation:** Google's free, keyless Maps Directions URL scheme.
- Each Overpass request now has a 28-second timeout with automatic mirror fallback, plus a Cancel button on the hunting spinner and a results summary once it finishes.

## A note on "still missing stores"
If a specific real-world location keeps not showing up after this fix, the most likely remaining explanation is that it simply isn't mapped in OpenStreetMap yet (or is tagged in some third way beyond `name`/`brand` that a regex search can't anticipate) — that's a data-completeness limit of the free service itself, not something more retrying can solve. The "Add Custom Store" option in Hunt Setup is the fallback for exactly this case.

## Reasonable next steps (not built yet)
- Actual Pro tier implementation (accounts, payment, real gating — today the free-tier caps are enforced but nothing is behind a real login).
- Cross-device sync (Export/Import in the ☰ menu covers manual transfer for now).
- True offline support — the manifest is a step toward a full PWA, but there's no service worker yet.
- hwtreasure.com integration for auto-loading TH/STH lists.

Happy hunting!
