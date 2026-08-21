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
  CadNSDI** database across eleven public-land states: California, Arizona,
  Nevada, Oregon, Washington, Idaho, Utah, Montana, Wyoming, Colorado, and
  New Mexico.
- Handles finer splits below the standard 40-acre quarter-quarter — a 20-acre
  half of a quarter-quarter (e.g. S½ NW¼ NE¼) or a 10-acre quarter of a
  quarter-quarter (e.g. NW¼ NE¼ SW¼) — by computing them via the standard
  BLM/GLO protraction method when BLM doesn't map that finer piece as its own
  polygon (it usually doesn't; only the 40-acre quarter-quarter is typically
  independently surveyed).
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
WGS84 can differ by roughly 1–2 meters depending on region (more pronounced
in tectonically active areas like California and the Pacific Northwest due
to plate motion).

## Scope / limitations

- Covers eleven public-land states and their principal meridians:
  - California — Humboldt, Mount Diablo, San Bernardino
  - Arizona — Gila and Salt River, Navajo, San Bernardino
  - Nevada — Mount Diablo
  - Oregon & Washington — Willamette
  - Idaho — Boise
  - Utah — Salt Lake, Uintah
  - Montana — Montana Principal
  - Wyoming — 6th Principal, Wind River
  - Colorado — 6th Principal, New Mexico Principal, Ute
  - New Mexico — New Mexico Principal

  Adding more states/meridians is straightforward — the BLM service and
  field structure are the same nationwide, it just needs the right
  `PRINMERCD` codes (verified live against BLM's PLSS Township layer) added
  to the meridian list and the state dropdown.
- Fractional/duplicate townships near meridian and baseline lines aren't
  handled by the simple form.
- Locates *aliquot-part* corners (quarter and quarter-quarter sections) only.
  Government Lots and metes-and-bounds parcels along rivers/shorelines
  aren't aliquot parts and won't be found — the tool's error message will
  say so if your section is lotted.
- A few principal meridians span more than one state (e.g. Mount Diablo
  covers both California and Nevada). The free-text parser flags this and
  defaults to a reasonable guess, but always double-check the State field
  matches your claim.

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

Eclipse Public License - v 2.0 (EPL-2.0). SPDX-License-Identifier: `EPL-2.0`.

Before publishing, add a `LICENSE` file to the repo containing the full
EPL-2.0 text from the canonical source: https://www.eclipse.org/legal/epl-2.0/
(a tool-level restriction on this session prevented reproducing the full
legal text verbatim here, so copy it directly from that link rather than
from this README).
