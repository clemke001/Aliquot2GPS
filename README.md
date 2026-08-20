# Aliquot Parts to GPS Calculator

A free, single-file web tool that turns a PLSS (Public Land Survey System) legal
description — Meridian, Township, Range, Section, Quarter/Quarter-Quarter — into
GPS coordinates for any corner of that parcel, using the Bureau of Land
Management's official live survey data instead of idealized 1-mile-square
section math.

No build step, no dependencies, no server. It's one HTML file you open in a
browser.

## What it does

- Looks up real, surveyed aliquot-part boundaries from BLM's live **PLSS
  CadNSDI** database for California's three principal meridians (Humboldt,
  Mount Diablo, San Bernardino).
- Returns GPS coordinates for any corner (NE/NW/SE/SW) or the center of the
  parcel, in decimal degrees and DMS.
- Lets you assemble a claim that spans multiple sections by calculating each
  aliquot part and adding it to a running claim list.
- Exports the whole claim as **GPX** (Gaia GPS and most GPS apps) or **KML**
  (Google Earth, and importable into Gaia GPS too) — including every corner,
  the center, and each parcel's actual surveyed boundary as a track/polygon.

## Why it's more accurate than the math

Real PLSS sections aren't perfect squares — convergence, historical survey
error, and terrain closures mean corners rarely fall exactly where simple
geometry predicts. This tool queries BLM's PLSS Intersected layer for the
actual surveyed/GCDB boundary of your specific aliquot part and reads real
corner vertices from it, rather than computing an idealized rectangle from
Township/Range numbers.

## Running it

Open `aliquot-to-gps-calculator.html` in any modern browser. That's it. It
queries `gis.blm.gov` directly from your browser (first via a standard
request, falling back to JSONP if that's blocked), so it needs normal
internet access — it won't work inside a sandboxed preview/iframe that blocks
outbound network requests.

## Accuracy notes

BLM CadNSDI positional accuracy varies by county and survey vintage —
typically better than a few meters in well-resurveyed areas, occasionally
coarser in remote or older-survey areas. Coordinates are returned in the
service's NAD83 datum reprojected to WGS84 (EPSG:4326); NAD83 and current
WGS84 can differ by roughly 1–2 meters in California due to tectonic drift.

## Scope / limitations

- California only for now (Humboldt, Mount Diablo, San Bernardino
  Meridians). Extending to other states/meridians is straightforward — the
  BLM service and field structure are the same nationwide, it just needs the
  right `PRINMERCD` codes and a places to plug in the meridian dropdown.
- Fractional/duplicate townships near meridian and baseline lines aren't
  handled by the simple form.
- Locates *aliquot-part* corners (quarter and quarter-quarter sections) only.
  Government Lots and metes-and-bounds parcels along rivers/shorelines
  aren't aliquot parts and won't be found — the tool's error message will
  say so if your section is lotted.

## Data source & credit

All coordinates come live from the Bureau of Land Management's **PLSS
CadNSDI** (Cadastral National Spatial Data Infrastructure) map service at
`gis.blm.gov`. Thanks to the BLM Cadastral Survey program for maintaining and
publishing this data publicly — none of this works without it.

## Disclaimer

**For educational and informational purposes only.** This tool is provided
as-is, with no warranty of any kind. It is not a substitute for a licensed
land survey. Always independently verify any coordinates — against official
BLM records and, where it matters, a licensed surveyor — before relying on
them for legal, financial, or safety-critical decisions, including staking
mining claims or locating property boundaries.

## License

Not yet decided — pick one before publishing (MIT is a reasonable default for
a free permissive tool like this: add a `LICENSE` file with your name and
the current year).
