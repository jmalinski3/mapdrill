# NFD Map Drill

A still-district driving drill for the Naperville Fire Department.

Pick your station → a random address in that still district comes up → study the
district map for a few seconds → drive a rig from the station to the address on
the real street network. The round ends when you arrive on scene.

## Using it

Everything is one file: **`index.html`**. Copy it anywhere (shared drive, desktop,
email attachment — you can rename it, e.g. `Map Drill.html`) and open it in any
modern browser (Chrome/Edge/Firefox/Safari). No install, no account.

It needs **internet access** for the base map, and the **first time each district
is loaded** it downloads that district's addresses and streets (a few MB) and
caches them in the browser, so later rounds start instantly.

### Controls

| Input | Action |
| --- | --- |
| `↑` / `W` | gas |
| `↓` / `S` | brake, then reverse while held at a stop |
| `◀` / `▶` (or `A`/`D`) | signal a turn for the next intersection |
| `U` | U-turn (when slow) |

The rig follows the streets on its own — you only choose where to turn. At a
T-intersection it stops and waits for a signal. On-screen pedals appear on
touch devices.

Two practice options on the menu:

- **Show the address pin while studying the district** (on by default) — turn it
  off to force pure street-name knowledge.
- **Practice mode** — keeps the pin visible while driving.

During a drive, **Reveal pin** gives up the location without ending the round.

## Where the data comes from

| Data | Source |
| --- | --- |
| Still-district boundaries | City of Naperville Open Data (*Fire Station Boundaries*) |
| Addresses | City of Naperville Open Data (*Address Points*) |
| Streets & base map | © OpenStreetMap contributors |

The app looks up the city's ArcGIS services at runtime, so it keeps working if
service URLs shift. District boundaries reflect whatever the city currently
publishes — when boundaries change, open **Diagnostics → Clear cached data &
reload** to pull fresh copies.

If the city ever reorganizes its open-data portal completely, open `index.html`
in a text editor, find the `CONFIG` block (search for `boundariesServiceUrl`),
and paste the new FeatureServer layer URLs there.

Station locations (drive start points) are hard-coded in the same file under
`STATION_COORDS` — edit there if a station moves.

## Tuning

All gameplay knobs are in the `CONFIG` block near the top of the app script in
`index.html`: preview countdown (`previewSeconds`), arrival radius
(`arrivalRadiusM`), top speed (`maxSpeed`), etc.

## Troubleshooting

- **Stuck on the first loading screen** — no internet, or the city/OSM services
  are unreachable. Details are under **Diagnostics** (top-right).
- **Blank gray map but the game works** — map tiles blocked; check firewall.
- **Weird/duplicate data after a city update** — Diagnostics → Clear cached
  data & reload.

## Development notes

`index.html` is self-contained: Leaflet 1.9.4 (BSD-2-Clause) is inlined, followed
by the app code (readable, commented) in the last `<script>` block. The driving
engine builds a routable street graph from OpenStreetMap ways (split at shared
nodes, one-ways respected, disconnected fragments dropped) and snaps each
address to the nearest street with a matching name. A test mode
(`?test=1` + injected `window.__TEST_DATA__`) exposes `window.__MD__` hooks for
automated tests.
