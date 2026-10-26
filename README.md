# Digital Powerline Assets

## Overview

A GPU-accelerated demo for Canadian utility owners to visualize and manage power transmission tower assets in a parametric 3D BIM + GIS environment. The demo is set in the Alberta Rocky Mountain foothills, west of Nordegg, along a 240 kV transmission corridor with 6 towers.

## Demo Features

### 1. GIS Map (Center Panel)
- **MapLibre GL** interactive map with terrain-style liberty basemap
- Tower markers color-coded by type (Tangent = green, Angle = amber, Dead-end = red)
- Power line corridor rendered as halo + main line
- Crossing objects (roads, rivers, power lines, buildings, trees) marked with orange markers
- Click any tower on the map to load its 3D model and fly to its location
- Metric scale control, navigation control

### 2. Parametric 3D Tower BIM (Right Panel)
- **Three.js r160** with ES Module + importmap (browser-native, no bundler needed)
- **GPU acceleration** via `powerPreference: 'high-performance'` — forces discrete GPU usage
- Full parametric lattice tower model:
  - 4 tapered main legs (angle steel)
  - X-pattern diagonal bracing on 4 faces across 5 body segments
  - 3-level cross arms (cat-head configuration): lower phase arms, center phase arm, ground wire supports
  - Insulator strings (disc-type, color-coded by tower type: suspension vs. strain)
  - 4-bundle conductors with catenary sag curves
  - Ground wires (OPGW) with sag
  - Foundation with 4 concrete piers
  - Tower label sprite (ID, type, model, heights)
  - Compass indicator
- **Mountain backdrop** — silhouette cones simulating Alberta Rocky Mountain foothills
- OrbitControls: drag to rotate, scroll to zoom, right-click to pan
- ACES Filmic tone mapping + sRGB color space for photorealistic rendering
- Shadow mapping with PCF soft shadows

### 3. BIM Dataset Browser (Left Panel)
9 data categories, each viewable as a scrollable data table:

| # | Dataset | Description |
|---|---------|-------------|
| 1 | Tower Survey | Per tower: ID, coordinates, elevation, type, dimensions, taper, angle |
| 2 | Conductors | Per span per phase: type, bundle, span length, sag, tension |
| 3 | Shield Wire / OPGW | Ground wire / OPGW type, height, sag, fibre count |
| 4 | Attachment Points | Insulator hanging point coordinates |
| 5 | Insulators & Hardware | String type, disc count, length, fittings |
| 6 | Member Specs | Main leg / bracing angle steel specs, material grade |
| 7 | Foundations | Type, depth, dimensions, anchor bolts, geotechnical |
| 8 | Weather Cases | Design temperature, wind, ice — sag calculation inputs |
| 9 | Crossings & Terrain | Crossing objects, ground elevations, clearance verification |

### 4. Tower Info Panel
Detailed specifications for the selected tower:
- Tower ID, type, coordinates (lon/lat), base elevation
- Model code, attachment height, overall height
- Base width, top width, taper ratio
- Line angle, cross-arm azimuth, phase arrangement, head type

## Technical Stack

| Component | Technology |
|-----------|-----------|
| 3D Engine | Three.js r160 (ES Module via importmap) |
| GIS Map | MapLibre GL JS v4.7.1 |
| Map Tiles | OpenFreeMap (liberty style) |
| Coordinate System | NAD83(CSRS) / UTM Zone 11N |
| Vertical Datum | CGVD2013 (Canadian Geodetic Vertical Datum 2013) |
| Rendering | WebGL2 with GPU acceleration (high-performance mode) |
| Tone Mapping | ACES Filmic |
| Shadow Type | PCF Soft Shadow Map |

## Data Standards

### Coordinate System
- **Horizontal**: NAD83(CSRS) / UTM Zone 11N (appropriate for Alberta, ~114°W to 118°W)
- **Vertical**: CGVD2013 (Canadian Geodetic Vertical Datum 2013)
- **Format**: Decimal degrees (DD) for map display; UTM Easting/Northing for engineering data

### Material Standards (Canadian)
- **Steel**: ASTM A36 (equivalent to Q235), ASTM A572 Gr.50 (Q345), ASTM A588 (Q420)
- **Bolts**: ASTM A325 / A490 (equivalent to M16/M20 high-strength)
- **Concrete**: CSA A23.1 25MPa / 30MPa
- **Anchor Bolts**: ASTM F1554 Gr.55

### Design Standards
- **CSA C22.3** — Overhead Transmission Line Inspection Standard
- **NBCC** — National Building Code of Canada (seismic design)
- **Transport Canada RPAS** — for UAV-based inspection operations

## Corridor Details

| Property | Value |
|----------|-------|
| Line Name | Alberta 240 kV Mountain Corridor |
| Voltage | 240 kV AC |
| Tower Count | 6 (T-101 through T-106) |
| Tower Types | Dead-end, Tangent, Angle |
| Conductor | ACSR Drake (4-bundle) |
| Shield Wire | OPGW-48B1-70 |
| Region | Rocky Mountain foothills, west of Nordegg, Alberta |
| Coordinates | ~115.84°W to 115.93°W, ~52.38°N to 52.44°N |
| Elevation Range | 1485 m to 1635 m |

## How to Run

```bash
# Option 1: Python http server
cd "C:\Users\LP\Desktop\Digital Powerline Assets"
python -m http.server 3000

# Option 2: Node.js
npx serve -p 3000
```

Then open `http://localhost:3000` in a browser with GPU acceleration enabled.

## File Structure

```
Digital Powerline Assets/
├── index.html                                          # Main demo application
├── Powerline_Tower_Parametric_3D_Modeling_Data_Template.xlsx  # English data collection template
├── README.md                                           # This file
└── data/
    ├── 01_tower_survey.csv                             # Tower survey & geometry (6 towers)
    ├── 02_conductors.csv                               # Conductor parameters (per span per phase)
    ├── 03_shield_wire_opgw.csv                         # Shield wire / OPGW parameters
    ├── 04_attachment_points.csv                        # Insulator attachment point coordinates
    ├── 05_insulators_hardware.csv                      # Insulator strings & hardware fittings
    ├── 06_member_specs.csv                             # Member specifications & material grades
    ├── 07_foundations.csv                              # Foundation parameters (per leg)
    ├── 08_weather_cases.csv                            # Weather conditions & calculation cases
    └── 09_crossings_terrain.csv                        # Crossing objects & terrain data
```

The demo loads all BIM data from the CSV files in `data/` at startup via `fetch()`. To update the data, simply edit the CSV files — no code changes needed.

## Data Collection Template (Excel)

The included Excel template (`Powerline_Tower_Parametric_3D_Modeling_Data_Template.xlsx`) contains 10 sheets:

1. **Instructions** — Scope, coordinate system, accuracy requirements, conventions
2. **Tower Survey** — One row per tower with geometry data
3. **Conductors** — Per span per phase conductor parameters
4. **Shield Wire / OPGW** — Ground wire and OPGW parameters
5. **Attachment Points** — Insulator hanging point coordinates
6. **Insulators & Hardware** — String types, disc counts, fittings
7. **Member Specs** — Angle steel / tube specifications and material grades
8. **Foundations** — Foundation type, dimensions, anchor bolts
9. **Weather Cases** — Design temperature, wind, ice for sag calculations
10. **Crossings & Terrain** — Crossing objects, clearances, terrain data

### Field Conventions
- **Red text** = Required field
- **Green text** = Optional field
- Enter numeric values only in numeric columns
- Refer to the "Unit" column for measurement units
