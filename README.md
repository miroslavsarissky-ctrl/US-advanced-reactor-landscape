# US Advanced Reactor Landscape Dashboard

Interactive market-intelligence dashboard mapping U.S.-relevant advanced reactor vendors, announced projects, candidate sites, national laboratories, and a strategic Aiken / Savannah River Site anchor.

The application is a **single-file HTML dashboard**. The production entry point is:

```text
index.html
```

No build step is required. The page can run directly from GitHub Pages or from a local browser.

---

## Live dashboard

Once GitHub Pages is enabled for this repository, the dashboard should be available at:

```text
https://miroslavsarissky-ctrl.github.io/US-advanced-reactor-landscape/
```

If the link does not load, check that GitHub Pages is enabled under:

```text
Settings → Pages → Build and deployment → Deploy from branch → main / root
```

---

## What the dashboard does

The dashboard provides a searchable, filterable map of the U.S. advanced reactor landscape, including:

- Vendor headquarters and operating centers
- Projects under construction or in early site work
- Planned or announced reactor projects
- Candidate federal and military sites under consideration
- DOE / NNSA national laboratories and related facilities
- Aiken, South Carolina / Savannah River Site as a strategic anchor

It is designed for executive briefings, market scanning, competitive intelligence, and peer-comparison work.

Useful questions it helps answer:

- Which advanced reactor technologies are clustered around INL, Oak Ridge, Texas, California, or the Southeast?
- Which projects depend on HALEU, TRISO, molten-salt fuel, or closed-fuel-cycle narratives?
- Which projects are military / federal, data-center-linked, fast-spectrum, or fuel-cycle-relevant?
- Where does an Aiken / SRS strategy sit relative to U.S. advanced reactor activity?

---

## Repository structure

Current lightweight structure:

```text
US-advanced-reactor-landscape/
├── index.html      # Dashboard application, styles, data, and JavaScript
└── README.md       # Project documentation
```

The dashboard data is currently embedded directly inside `index.html` as JavaScript arrays. This keeps the project simple and portable.

Potential future structure, if the dashboard grows:

```text
US-advanced-reactor-landscape/
├── index.html
├── README.md
├── data/
│   ├── vendors.json
│   ├── projects.json
│   ├── labs.json
│   └── focal.json
└── src/
    └── dashboard.js
```

---

## How to run locally

### Option 1: Open directly

Download or clone the repository and open:

```text
index.html
```

in Chrome, Edge, Safari, or Firefox.

### Option 2: Run a simple local server

This is useful if the browser blocks local assets or if future versions use separate JSON files.

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/
```

---

## External dependencies

The dashboard uses external map libraries and map tiles:

- Leaflet
- CARTO / OpenStreetMap dark basemap tiles

Internet access is required for the map tiles and CDN-hosted Leaflet assets to load correctly.

---

## Main controls

### Search

Search across vendor name, project name, design, site, address, and selected metadata.

Example searches:

```text
Aiken
HALEU
TRISO
INL
Google
fast spectrum
EAGL-1
```

### Marker type filters

| Filter | Meaning |
|---|---|
| Vendor HQ | Company headquarters or principal listed operating address |
| Construction / site work | Publicly announced construction, groundbreaking, or site-work activity |
| Planned / announced | Announced project or deployment plan, but not yet clearly in construction |
| Candidate / considered | Sites selected for consideration, but not final deployment decisions |
| National lab | DOE / NNSA national laboratories or related facilities |
| Aiken / SRS | Strategic anchor marker |

### Technology filters

Technology is primarily based on coolant or broad reactor family:

- Light water
- High-temperature gas reactor, or HTGR
- Molten salt
- Sodium-cooled
- Lead / lead-bismuth
- Other / TBD

### Fuel assay filters

Assay and fuel form are intentionally separated.

| Filter | Meaning |
|---|---|
| LEU | Low-enriched uranium, typically below 5% U-235 |
| HALEU | High-assay low-enriched uranium, 5–20% U-235 |
| Assay TBD | Assay not clearly identified in public sources |

### Fuel form filters

| Filter | Meaning |
|---|---|
| Oxide / ceramic | Solid oxide or ceramic fuel forms |
| TRISO | Coated-particle TRISO fuel, usually HALEU-based for advanced reactor designs |
| Metallic / alloy | Metallic fuel or alloy fuel |
| Molten fissile salt | Liquid fuel dissolved in molten salt |
| Fuel form TBD | Fuel form not publicly clear |

### Size class filters

| Filter | Definition used in dashboard |
|---|---|
| Microreactor | ≤20 MWe per unit |
| SMR / small modular | >20 MWe and ≤300 MWe per unit |
| Above-SMR | >300 MWe per unit |
| Research / non-power | Research reactor, thermal-only, or non-electric demonstration |
| TBD / unspecified | Public data insufficient |

### Strategic filters

The dashboard includes quick strategic filters for:

- Fast spectrum
- Closed fuel cycle
- HALEU dependency
- Military / federal projects
- Data-center / hyperscaler-linked projects

These filters are for market-intelligence triage, not formal regulatory classification.

---

## Export functions

The dashboard includes export buttons for the currently visible marker set.

| Button | Output |
|---|---|
| Export visible CSV | Downloads currently visible markers as a CSV file |
| Export visible JSON | Downloads currently visible markers as JSON |
| Copy visible markers | Copies currently visible marker data to clipboard |

Use filters first, then export a focused subset such as:

- HALEU-dependent projects
- Military / federal pilots
- INL cluster projects
- Fast-spectrum projects
- Data-center-linked projects

---

## Data model

The dashboard data is embedded in `index.html` as JavaScript arrays.

| Array | Contents |
|---|---|
| `VENDORS` | Vendor / developer entities |
| `PROJECTS` | Reactor projects, demonstrations, candidate sites, and deployment plans |
| `LABS` | National laboratories and related DOE / NNSA facilities |
| `FOCAL` | Strategic anchor marker, currently Aiken / SRS |

Typical marker object:

```js
{
  name: "Project or vendor name",
  design: "Reactor design",
  capacity: "Capacity or thermal/electric rating",
  fuel: "Human-readable fuel description",
  site: "Site or address",
  lat: 00.0000,
  lon: -00.0000,
  status: "planned",
  tech: ["sodium"],
  size: ["smr"],
  assayKeys: ["haleu"],
  formKeys: ["metal"],
  strategic: ["fast", "haleu"],
  sourceType: "Company / NRC / DOE / EIA",
  sourceDate: "Verified April 2026",
  sourceUrl: "https://...",
  confidence: "High | Medium | Low",
  caveat: "Important qualification for investor use."
}
```

### Key enums

#### `tech`

```js
lw, htgr, ms, sodium, lead, other
```

#### `size`

```js
micro, smr, large, nonpower, tbd
```

#### `assayKeys`

```js
leu, haleu, tbd
```

#### `formKeys`

```js
oxide, triso, metal, mfs, tbd
```

#### `status`

```js
vendor, uc, planned, candidate, lab, focal
```

#### `strategic`

```js
fast, closed, haleu, military, datacenter
```

---

## Important caveat

This dashboard is a **market-intelligence map**, not a licensing-status register.

Project status may combine materially different pathways, including:

- NRC licensing
- DOE authorization
- Department of Defense or Department of the Air Force procurement
- Company announcements
- Site-selection or land-option announcements
- Early-stage pre-application engagement

Capacity, fuel form, deployment date, waste-reduction claims, schedule, closed-fuel-cycle claims, and commercial-readiness claims should be treated as **developer-stated unless independently confirmed**.

Before using the dashboard in investor, legal, SEC, board, or external materials, verify each relevant marker against primary sources.

---

## Notable data-quality updates in this version

This version includes several corrections and investor-grade caveats versus the earlier draft:

- FANCO / EAGL-1 changed from generic TBD fuel to **HALEU dioxide initially**, with closed-cycle claims caveated.
- FANCO Indiana energy park waste-reduction language softened to **95–97% developer-stated**.
- Hermes 2 moved from “planned” to **construction / site work** following public groundbreaking.
- ACU MSR-1 corrected to **1 MWt, non-power research reactor**, not an electric-output project.
- Eielson Air Force Base microreactor added as a federal / military pilot marker.
- Janus Program sites moved into **candidate / under consideration**, rather than fully planned deployment sites.
- Fuel taxonomy separated into **assay** and **fuel form**.
- Size taxonomy expanded to include **above-SMR** and **research / non-power** categories.
- Per-marker source, confidence, and caveat fields added to popup metadata.

---

## Maintenance workflow

### 1. Add or update source evidence

Prefer primary or high-quality sources:

1. NRC pages, ADAMS filings, application documents, and safety evaluations
2. DOE / NNSA / national laboratory releases
3. Federal Register notices
4. SEC filings
5. State economic-development announcements
6. Company press releases
7. Credible trade press, only where primary source is unavailable

### 2. Update the relevant JavaScript object

Open `index.html` and find the relevant array:

- Vendor: `VENDORS`
- Project: `PROJECTS`
- Lab: `LABS`
- Strategic marker: `FOCAL`

Then update the relevant fields:

- `status`
- `tech`
- `size`
- `assayKeys`
- `formKeys`
- `strategic`
- `sourceDate`
- `sourceUrl`
- `confidence`
- `caveat`

### 3. Test locally

Open the dashboard and test:

- All marker types render
- Search works
- Technology filters work
- Assay filters work
- Fuel form filters work
- Size filters work
- Strategic filters work
- Export buttons work
- Right-side visible list updates correctly
- Popups show source, confidence, and caveat fields correctly

### 4. Validate investor-facing claims

For each new or changed item, check:

- Is this a confirmed project, candidate site, or developer aspiration?
- Is the capacity electric, thermal, or both?
- Is the fuel form known, or only the enrichment range?
- Is the regulatory pathway NRC, DOE, DOD, or unclear?
- Is the site precise, approximate, or only a regional centroid?
- Is the statement independently confirmed, or company-stated?

---

## Recommended next improvements

Suggested next development steps:

1. Add marker clustering for dense regions such as INL, Oak Ridge, California, Texas, and the D.C. region.
2. Add a source-register panel or downloadable source table.
3. Add a date-based `lastVerified` field distinct from source publication date.
4. Add deployment timeline fields: announced date, target construction, target operation, regulatory milestone.
5. Add licensing-path fields: NRC, DOE, DOD, foreign regulator, TBD.
6. Add confidence coloring in marker popups.
7. Add comparison mode for selected vendors or technologies.
8. Add a newcleo-specific comparison layer for LFR, MOX, closed fuel cycle, Aiken / SRS, and federal fuel-cycle relevance.

---

## Suggested governance

For internal use, treat this dashboard as a living market-intelligence product.

Recommended owner model:

- **Content owner:** Market Intelligence / Strategy
- **Technical owner:** Communications / Digital or designated dashboard maintainer
- **Fact-check reviewer:** Regulatory / Licensing for licensing claims; Fuel Cycle team for fuel claims
- **Update cadence:** Monthly, with ad hoc updates for major NRC, DOE, SEC, or company announcements
- **Use restriction:** Do not use externally without final source review and legal/comms sign-off

---

## License / usage note

This dashboard is for market-intelligence and strategic analysis. Source data comes from public materials, but dashboard classification, caveats, and strategic tags reflect analytical judgement and should be reviewed before external distribution.
