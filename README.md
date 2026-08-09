# TransitOps — Fleet Control Center

A single-file, browser-based dashboard for tracking a live campus/city bus fleet on a map and publishing new routes. Built with Tailwind CSS, Leaflet (OpenStreetMap), and OSRM for road-snapped routing — no build step, no backend required.

## Quick start

Open `transitops.html` directly in any modern browser (Chrome, Safari, Firefox, Edge). That's it — all dependencies load from public CDNs, and vehicle positions are simulated client-side.

No install, no server, no API keys.

## What it does

- **Live tracking view** — shows simulated buses moving along real, road-snapped routes on a map, with speed, ETA, next stop, and capacity shown for the selected vehicle.
- **Route management view** — a form to publish a new route (name, vehicle ID, start/end coordinates, target speed). The route is automatically snapped to the actual road network and a new bus is deployed and animated immediately.
- **Search** — filter the fleet list by vehicle ID or route name.
- **Simulation speed control** — 0.5×–4× so you can watch buses complete a loop quickly for demos.
- **Emergency broadcast** — a mock "notify all drivers/dispatch" action with a confirmation modal.
- **Fully responsive** — a sidebar on desktop, a swipeable bottom sheet on mobile.

## How it works

| Piece | Role |
|---|---|
| **Leaflet.js** | Renders the map, tile layer, markers, and route polylines. |
| **CARTO Dark Matter tiles** | Basemap styled to match the dark control-room theme. |
| **OSRM demo routing server** (`router.project-osrm.org`) | Given a start/end point, returns a real driving path; the app animates buses along that path rather than a straight line. |
| **`requestAnimationFrame` loop** | Moves each bus along its route each frame, based on elapsed time and its target speed — this is a simulation, not real GPS. |
| **Tailwind CSS (CDN/Play mode)** | All layout and utility styling. |
| **Font Awesome** | Icons. |

Everything — fleet data, animation state, form handling — lives in memory in the browser tab. Refreshing the page resets the fleet to the two default buses.

## Adding a route manually

1. Switch to the **Routes** tab.
2. Fill in a route name, a unique vehicle ID, start/end coordinates as `lat, lng` (decimal degrees), and a target speed.
3. Submit — the app calls OSRM to snap the path to roads, then deploys and selects the new bus.

If OSRM is unreachable (offline, rate-limited, or blocked by a network policy), the app falls back to a straight line between the given points and shows a small toast notice rather than failing silently.

## Customizing

Everything is in one file, organized top to bottom as: `<head>` (CDN links, theme CSS) → HTML structure → inline `<script>` (app logic).

- **Colors / theme** — edit the CSS custom properties in `:root` at the top of the `<style>` block (`--accent-cyan`, `--accent-amber`, `--bg-deep`, etc.).
- **Default fleet** — edit the `buses` array near the top of the `<script>` block to change starting vehicles, routes, or stops.
- **Map center/zoom** — change the `setView([lat, lng], zoom)` call where the Leaflet map is initialized.
- **Capacity thresholds / colors** — search for `capacityNum > 85` / `> 65` to adjust the green/amber/red cutoffs.

## Known limitations

- Positions are simulated, not real GPS — this is a demo/prototype, not a production tracking system.
- OSRM's public demo server has no uptime guarantee and isn't meant for production traffic; for real deployments, swap in your own routing provider or a self-hosted OSRM instance.
- No persistence — added routes and fleet state are lost on page refresh.
- No authentication — anyone with the file can view and use it; add your own access control before deploying publicly.

## Browser support

Built and tested against current versions of Chrome, Safari, Firefox, and Edge, on both desktop and mobile viewports. Requires JavaScript enabled.
