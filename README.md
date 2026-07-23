# Set & Drift

A single-file, no-install sailing ETA and route planner. Pick a start and end point, a departure time, and a boat, and it works out a realistic arrival time from live wind, wave, and current forecasts — not just distance ÷ hull speed.

Built for planning trips around the Danish straits (Öresund, Kattegat approaches, Bornholm), but the weather math works anywhere; only land-avoidance is limited to that region (see [Coverage area](#coverage-area) below).

**[Open the live app](#)** *(replace with your GitHub Pages URL)*

---

## Features

- **Two ways to set a route**: upload a GPX file, or tap start/end points directly on a map.
- **Two routing modes**:
  - *Follow the uploaded route exactly* — simulates your own waypoints leg by leg.
  - *Find fastest route* — an isochrone search that finds the fastest path between two points, tacking around wind angles and steering clear of land automatically.
- **Real weather, not averages**: wind, gusts, wave height, and current are fetched per-location and per-time along the whole route, from [Open-Meteo](https://open-meteo.com/) (free, no API key).
- **Boat polars**: a generic 35ft cruiser, and an estimated polar for a Kitt 25 motorsailer, tuned from real hull-ratio data (SA/D, hull speed) rather than a guess. Boat speed is looked up from wind speed and true wind angle, not assumed constant.
- **Motor-sailing mode**: optionally models an engine that kicks in whenever sailing would be slower — including dead upwind — at a speed you set.
- **Real land avoidance**: routing checks candidate paths against actual embedded land polygons (point-in-polygon and exact segment-intersection testing), not a heuristic guess. See [Coverage area](#coverage-area).
- **Departure optimizer**: sweeps the next 48 hours of forecasts and charts trip duration by departure time, so you can see the fastest weather window at a glance.
- **Conditions summary**: peak wind, gusts, and wave height along the route, with warnings for anything rough.
- **Live wind overlay** on the results map (toggleable), plus an OpenSeaMap seamark layer for navigational context.
- **Snapshot export**: save the route, ETA, and conditions as a downloadable PNG to share.
- **Light/dark theme**, expandable maps, and a mobile-friendly layout.

## Quick start

1. Open the app.
2. Set a route: upload a `.gpx` file, or switch to "Tap on map" and place a start and end point.
3. Pick a departure date/time and a boat.
4. Choose a routing mode.
5. Hit **Calculate ETA** (or **Find best departure** to scan the next 48 hours first).

## Coverage area

Land avoidance uses a land dataset embedded directly in the app (~9.5–15.6°E, 53.8–56.8°N) — roughly Danish waters, the Öresund, Bornholm, and the German/Polish Baltic coast. Outside that box, the router has no land data to check against and will not detect coastline. The weather and boat-speed math work anywhere in the world; only this land-avoidance piece is regional.

To extend coverage to a different area, a new region can be clipped from a world land-boundary source (see [Data & attribution](#data--attribution)) and swapped into the embedded dataset.

## How the routing works (short version)

"Find fastest route" spreads an expanding wavefront of reachable points outward from the start (an isochrone search), testing many possible headings against the wind and current at each point in time, and keeping the furthest-advanced candidates in each direction. Any candidate move that would cross land is rejected using exact point-in-polygon and segment-intersection tests against the embedded coastline data. This is what lets it find tacking routes through wind angles a straight line can't sail, and route around islands and peninsulas automatically.

## Limitations, honestly

- **Boat polars are estimates**, not measured performance — especially the Kitt 25, which has no published polar and is built from hull-ratio formulas. Treat ETAs as a planning aid, not a guarantee.
- **Land data is planning-resolution, not chart-grade.** It can miss very narrow channels, harbor breakwaters, or piers not present in the source data. Always cross-check a suggested route against a real chart before sailing it.
- **No depth or hazard data.** The router knows land vs. water, not water depth, charted wrecks, restricted areas, or traffic separation schemes.
- **No official routing authority.** This is a personal planning tool, not a substitute for proper passage planning, local knowledge, or navigational judgment.

## Data & attribution

- Weather, wave, and current forecasts: [Open-Meteo](https://open-meteo.com/) (CC BY 4.0)
- Base map tiles: [CARTO](https://carto.com/attributions) / [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors
- Seamark overlay: [OpenSeaMap](https://www.openseamap.org/) contributors
- Land boundary data: derived from a world countries boundary dataset, clipped and repaired for this region
- Mapping library: [Leaflet](https://leafletjs.com/)

## Tech notes

Single HTML file, no build step, no backend. Runs entirely in the browser and can be hosted for free on GitHub Pages — just push `index.html` (or whatever this file is named in your repo) and enable Pages in the repo settings.

## Disclaimer

This tool is for trip planning and general reference only. It is not a certified navigation aid. Always cross-check routes against official nautical charts, current Notices to Mariners, and your own judgment before sailing.
