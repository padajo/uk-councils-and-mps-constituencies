# UK Council Boundaries

An outline map of UK local government and Westminster constituency boundaries, built to show how the tiers overlap. It is a static site (Leaflet plus two GeoJSON files) that runs on GitHub Pages with no build step.

## What it shows

| Layer | Contents | Count |
|---|---|---|
| County councils | Top tier in two-tier areas of England | 21 |
| District councils | Lower tier in two-tier areas of England | 164 |
| Unitary and single-tier | Unitaries, metropolitan and London boroughs, and all Scottish, Welsh and Northern Irish councils | 197 |
| Constituencies | Westminster parliamentary constituencies (July 2024) | 650 |

Each layer is drawn as an outline with no fill, so layers can sit on top of each other. Click anywhere on the map to see which county, district, unitary and constituency that point falls in.

Boundaries are generalised (500m), so they are approximate. Do not use them for precise boundary work.

## Repo layout

```
docs/
  index.html                       The map page
  uk_local_government_tiers.geojson County, district and unitary layers
  uk_constituencies.geojson        Westminster constituencies
```

## Run it

Publish with GitHub Pages: Settings > Pages > Deploy from a branch > branch `main`, folder `/docs`.

To run it locally, serve the `docs` folder over HTTP, because browsers block `fetch` from `file://`:

```
cd docs
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Data

`uk_local_government_tiers.geojson` has one feature per area with these properties:

| Property | Meaning |
|---|---|
| `code` | ONS area code, for example `E10000016` |
| `name` | Area name |
| `tier` | `county`, `district` or `unitary` |
| `subtype` | `county council`, `district council`, `unitary authority`, `metropolitan borough`, `London borough`, `Scottish council`, `Welsh council` or `NI council` |
| `source` | Which ONS dataset the feature came from |

`uk_constituencies.geojson` has `code` and `name` for each constituency.

All coordinates are WGS84 (EPSG:4326).

### How the local government file was built

County councils and single-tier areas come from the ONS Counties and Unitary Authorities (December 2025) BUC file. District councils (codes starting `E07`) come from the ONS Local Authority Districts (May 2025) BUC file. Single-tier areas appear in both ONS files, so they were taken once from the counties file. The two files had identical sets of single-tier areas, so nothing was lost in the merge.

### Sources

- ONS Open Geography Portal: https://open-geography-portalx-ons.hub.arcgis.com
- Counties and Unitary Authorities (December 2025), BUC
- Local Authority Districts (May 2025), BUC
- Westminster Parliamentary Constituencies (July 2024), BUC

BUC means ultra generalised (500m), clipped to the coastline.

Actual data sources used for the current version:

- [Counties and Unitary Authorities (December 2025) BUC](https://open-geography-portalx-ons.hub.arcgis.com/datasets/ons::counties-and-unitary-authorities-december-2025-boundaries-uk-buc)
- [Local Authority Districts (May 2025) BUC](https://open-geography-portalx-ons.hub.arcgis.com/datasets/ons::local-authority-districts-may-2025-boundaries-uk-buc-2)
- [Westminster Parliamentary Constituencies (July 2024) BUC](https://open-geography-portalx-ons.hub.arcgis.com/datasets/ons::westminster-parliamentary-constituencies-july-2024-boundaries-uk-buc-2)

## Updating the data

1. Download the newer GeoJSON files from the ONS Open Geography Portal (same BUC resolution).
2. Rebuild `uk_local_government_tiers.geojson`: keep `E10` codes as `county`, `E07` codes as `district`, and every other code as `unitary`, taking single-tier areas only once.
3. Replace the files in `docs/` and commit.

The two local government files have different vintages, so check the ONS release notes for any reorganisations between them.

## Adding your own layer

Drop any polygon GeoJSON onto the map to overlay it as the constituencies layer. This is useful for trying out a custom area set, such as local party areas built from whole councils or constituencies.

## Licence and attribution

Contains National Statistics data and Ordnance Survey data. Crown copyright and database right 2025. Used under the Open Government Licence v3.0: https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/

The map page uses Leaflet (BSD 2-Clause): https://leafletjs.com
