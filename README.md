# Treasure Hunt Tracker 🎯

> 🧪 **Status: ALPHA.** Core hunting/routing/tracking works end-to-end, but the app is actively growing (multi-category support just landed) and things will keep shifting. See the in-app ☰ menu → Version History for the changelog.

A mobile-first store tracker and route planner for collectible hunting — Die Cast, Trading Cards, Video Games, LEGO, and Pop Culture, with room to grow. Hosted on GitHub Pages — no accounts, no paid APIs, no backend.

## Multi-category hunting (v2.0-alpha)
Hunt Setup now starts with a **"What Are You Hunting For?"** dropdown (Die Cast / Trading Cards / Video Games / LEGO / Pop Culture). Changing it:
- Auto-checks the chain stores and local shop types relevant to that category — nothing is ever hidden or deleted, so you can still hand-pick individual chains afterward.
- Swaps the "Item Types to Track" list to that category's defaults (e.g. Silver Series/Premium/Pop Race for Die Cast vs. Pokémon TCG/Sports for Trading Cards), each with its own independently-saved custom list.
- Updates the Hot List's example placeholder so it matches what you're collecting.

**New chain stores:** Kroger (Grocery/Big Box); GameStop (Games & Hobby); Barnes & Noble and Books-A-Million (Books & Media); Five Below (Discount & Novelty); Best Buy (Electronics); Costco (Wholesale Clubs); Hot Topic, BoxLunch, and Spencer's (Pop Culture Specialty).

**Independent/Local Shops:** a new section searches by OpenStreetMap shop *type* rather than brand name — Comic Book Store, Card & Game Shop, Hobby Shop, Toy Store, Independent Bookstore, and Thrift/Secondhand Store — so local spots show up without you having to list every one by name. Coverage depends entirely on how completely each shop is mapped in OpenStreetMap; if a known local store doesn't turn up, it likely just isn't tagged there yet (anyone can add it at openstreetmap.org).

## On PriceCharting
Asked and answered honestly, same as the Mattel image question below: **a client-side PriceCharting scraper isn't something this app can reliably do.** Two separate walls, either one of which is a blocker on its own:
1. **Cross-origin restrictions.** Like Mattel's site, PriceCharting's pages aren't set up to let arbitrary browser JavaScript fetch and parse their HTML from another origin — the same technical wall documented below for images applies to price/listing data too.
2. **Terms of use.** PriceCharting's data is their business; programmatic access is meant to go through their paid API, not scraping. Building a scraper that routes around that would violate their terms regardless of whether it worked technically.

So it isn't in this build, and "scrape PriceCharting" specifically isn't a path to revisit — a *licensed* PriceCharting API key (a real, budgeted cost, which breaks the zero-cost-stack constraint this project is built around) would be the legitimate way to pull pricing data or refine category checklists, if that tradeoff is ever worth making.

## On pulling images from Mattel's own website
Asked and answered honestly: **switching the image source to Mattel's own site wouldn't actually solve the copyright concern**, so I didn't implement it. Copyright ownership is about who *created* the photo, not which website happens to display it — Mattel's product photography is exactly as copyrighted on hotwheels.com as it is on a collector site. There's also a practical wall regardless of copyright: most websites (including Mattel's) don't allow arbitrary cross-origin JavaScript to fetch their images or HTML, so a client-side app like this one can't reliably scrape either source even if it wanted to.

What I did instead: the "Import 2026 Treasure Hunts" Pro feature (added last round) uses real, search-verified car *names* only — names aren't meaningfully copyrightable the way product photography is — paired with the app's own logo as a placeholder image. You can always tap that placeholder to add your own photo once you find the car. If a legitimate licensed image API ever exists for this, it'd be a clean drop-in replacement; scraping isn't the path there.

## This round's changes

**Store cards & Hot List — real photo management, not just "add more":**
- Selecting a photo now shows a preview with explicit **💾 Save** / **✕ Cancel** buttons — nothing is written until you tap Save.
- Added **🔄 Replace** and **🗑 Remove** (with confirmation) for the current photo.

**Saved Stores reworked:**
- Action buttons (⭐ Favorites filter, 🔥 Favorite selected, 🗑 Delete selected) moved to the top of the list, above the checkboxes they act on.
- "Route from Saved Stores" renamed to **Route from Selected**, and it now clears any currently-displayed route/hunt from the map first so old and new results can't visually mix.
- **⭐ Favorites** button toggles a filter to show only your favorited stores in this list.

**Routing:**
- **Last-used Start/End now persists** across sessions, the same way other settings do.
- **Save/Load/Delete Route** added to Route Details — saves your Start/End configuration (not the store list itself) under a name for reuse.
- **Save/Load Hunt moved from the hamburger menu into Hunt Setup**, next to the settings it actually configures.

**Fixed: tab panels scrolling to the "middle" instead of the top.** Real bug, not cosmetic — the scroll was firing based on a `setTimeout` guess while the panel's slide-open CSS animation was still in progress, so it measured a half-expanded panel and landed short. Now it waits for the animation to actually finish (via `transitionend`, with a timeout fallback) before scrolling. Affected both Route Details and Hot List identically, so both are fixed the same way.

**Admin PIN unlock changed from 1 tap to 5 taps** within 2 seconds — a single tap/hold on a mobile image commonly triggers the browser's own "save image" context menu, which would fight with a single-click PIN prompt. Five quick taps avoids that collision entirely.

**Store list additions:** Speedway (defaults to Silver Series checked) joins 7-Eleven under "Convenience / Gas."

**Small stuff:** a note under the HUNT! button warning that large searches can take a few minutes; the hamburger menu now scrolls to its own top every time it opens.

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
- A licensed image source for the TH/STH import, if one ever becomes available — see the copyright note above for why that's the blocker, not effort.

Happy hunting!
