# AeroPave — Airport Pavement Analysis Web App

Interactive analysis tool for FAARFIELD-based pavement design assessment. Built
as the visualization layer for the **CEE 598 Airport Design Final Project**
(Arizona State University, Spring 2026), wrapping the FAARFIELD 2.1.1 engine
for 6 airports across 13 pavement sections.

## Live demo

Deploy this repo to **Netlify** (free tier) for a fully-functional report-only
view. Recommended Netlify settings (already encoded in `netlify.toml`):

- Build command: `npm run build`
- Publish directory: `dist`
- Node version: 20

Drag-and-drop deploy: run `npm run build` locally, then drop the `dist/` folder
at https://app.netlify.com/drop. Or connect this GitHub repo for auto-deploy
on every push.

## What works on Netlify (report-only mode)

- All 13 pre-calculated CDF verdicts (4 OVER / 9 UNDER)
- Per-section CDF profile charts (41-point lateral sweep)
- Gear footprint top-view (top 10 contributors, real-aircraft wheel coords)
- Per-aircraft contribution table with library gear authoritative labels
- Cross-section diagrams + frost-depth indicators
- ASTM D5340 PCI history charts (7 rating bands as reference areas)
- Distress breakdown bar charts (load / climate / other categorization)
- CDF-vs-PCI field validation scatter plot
- Frost penetration vs pavement section chart
- Per-airport detail panels with subgrade + traffic info

## What requires the live backend (NOT on Netlify)

- Live recompute when a slider changes (layer thickness, MOR, growth rate)
- LEAF stress contour (2D heatmap) for arbitrary inputs
- Stress-vs-depth profile for arbitrary inputs
- 3D FEM mesh viewer with sigma-tensor field
- Searched-airport analysis (any airport not in the 13 project sections)
- Live aircraft library lookup by ICAO

These features show "FAARFIELD backend offline" badges and disable cleanly when
the backend is unreachable. The analysis displayed for the 13 project airports
is the same data used in the final-project report — fully complete without any
live computation.

## Local development

```bash
npm install
npm run dev   # http://localhost:5173
```

For full local backend functionality, also install FAARFIELD 2.1.1 and run the
companion .NET wrapper. See the parent project repo at
[Pleesudjai/cee598-airport-final](https://github.com/Pleesudjai/cee598-airport-final)
for the FAARFIELD wrapper setup guide.

## Stack

- React 19 with Hooks
- Vite 8 (build / dev server)
- Tailwind CSS (utility-first styling)
- Recharts (line / bar / scatter charts)
- Plotly (3D mesh visualization, when backend is online)
- MapLibre (airport map view)
- Static JSON data layer in `src/data/`:
  - `cdf_results.json` — pre-cal'd CDF for all 13 sections (415 KB)
  - `aircraft_library.json` — enriched 1,310-record aircraft library
  - `pci_distress.json` — ASTM D5340 PCI inspections + distress records
  - `frost_data.json` — NOAA Climate Normals to modified Berggren frost depth
  - `subgrade.json`, `traffic.json`, `sections.json`, `airports.json`

## Methodology

CDF analysis follows FAA AC 150/5320-6G with three failure modes:

- AC fatigue (RDEC model)
- PCC fatigue (FAARFIELD 2014 model)
- Subgrade rutting (standard model)

PCI inspections follow ASTM D5340-12 (airport pavements; 7-band rating system:
Good 86-100, Satisfactory 71-85, Fair 56-70, Poor 41-55, Very Poor 26-40,
Serious 11-25, Failed 0-10). Frost depth follows the modified Berggren equation
per FAA AC 150/5320-6F Chapter 3.

The AeroPave site shows analyses verified by a 130-row gear-coordinate trace
audit (1e-6 inch tolerance, zero divergences) — wheel coordinates from the
enriched library reach the LEAF and CDF solvers unaltered for every aircraft
x section combination.

## Author

**Chidchanok Pleesudjai** — PhD Candidate, School of Sustainable Engineering
and the Built Environment, Arizona State University.

## License

This repository contains only the visualization frontend and pre-cal'd analysis
results. FAARFIELD itself is FAA-published software under separate FAA license;
the proprietary FAARFIELD DLLs are NOT redistributed in this repo. Project data
is from the FAA-published Project Data archive used in CEE 598.
