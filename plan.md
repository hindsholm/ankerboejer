# Plan: Ankerbøjer Interactive Map

## TL;DR
Create a single self-contained HTML file that renders an OpenLayers map showing all 24 mooring buoys from the CSV as yellow flag markers with name labels. All dependencies loaded from CDNs, no build step.

## Data
- 24 buoys in `ankerboejer.csv`
- Coordinates in degrees + decimal minutes (e.g. `56 38.315, 09 50.875`)
- Must be converted to decimal degrees (EPSG:4326) then projected to EPSG:3857 for OpenLayers
- Fields: Nr, Ejer (owner), Nord, Øst, Stednavn (place name)

## Steps

### Phase 1: Create HTML file
1. Create `/home/mohi/projects/anker/index.html` with:
   - OpenLayers CSS/JS from CDN (`https://cdn.jsdelivr.net/npm/ol@latest/dist/ol.js` and `.css`)
   - Inline all buoy data as a JS array (hardcoded from CSV — avoids needing a CSV parser or fetch)
   - Convert degree-minute coords to decimal degrees inline
   - OSM tile base layer
   - Vector layer with yellow flag icon + text label for each buoy
   - Yellow flag: use an inline SVG data URI (a small flag shape) — no external image dependency
   - Text style below/beside icon showing `Stednavn`
   - Map centered on the buoy cluster (around 56.67°N, 10.05°E) with appropriate zoom
   - Popup on click showing full details (Nr, Ejer, Stednavn)

### Phase 2: Styling
2. Yellow flag marker via inline SVG data URI — simple pennant/flag shape in yellow
3. Name labels using `ol/style/Text` with white halo for readability
4. Map fills the viewport (full-screen)

## Relevant files
- `ankerboejer.csv` — source data (read-only, embed data into HTML)
- `index.html` — to be created, the deliverable

## Verification
1. Open `index.html` in a browser — map loads with OSM tiles
2. 24 yellow flag markers visible in the Mariager Fjord area
3. Each marker shows its place name
4. Clicking a marker shows a popup with details

## Decisions
- Embed buoy data directly in JS rather than fetching/parsing CSV at runtime — simpler, no CORS issues
- Use inline SVG data URI for yellow flag — no external image dependency
- Single HTML file, no separate JS/CSS files
- OpenLayers loaded from jsDelivr CDN
