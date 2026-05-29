<div align="center">
  <img src="Thermato/icon/thermato.png" alt="THERMATO Logo" width="220"/>
  <h1>THERMATO</h1>
  <h3>Urban Heat Risk Analysis — QGIS Plugin</h3>

  <p>
    <img src="https://img.shields.io/badge/QGIS-3.x-green?logo=qgis&logoColor=white"/>
    <img src="https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white"/>
    <img src="https://img.shields.io/badge/License-GPL%20v2-orange"/>
    <img src="https://img.shields.io/badge/Status-Active-brightgreen"/>
  </p>
</div>

---

**THERMATO** is a QGIS Processing plugin for automated spatial analysis of urban heat risk. It integrates four geospatial layers — Land Surface Temperature (LST), Built-up Index (NDBI), Vegetation Index (NDVI), and Population Density (POP) — into a single composite urban heat risk index, following a weighted multi-criteria framework. All rasters are automatically resampled to a common reference grid, validated strictly, and output as continuous raster, classified raster, vector polygon, and a full HTML report.

> Developed in support of urban climate research and spatial planning in Indonesia, in compliance with the national guidelines framework of the Ministry of Environment and Forestry.

---

## Table of Contents

- [Features](#features)
- [Input Parameters](#input-parameters)
- [Output Structure](#output-structure)
- [Risk Classification](#risk-classification)
- [Component Default Scores](#component-default-scores)
- [Methodology](#methodology)
- [Requirements](#requirements)
- [Installation](#installation)
- [Authors](#authors)

---

## Features

- **Four-component risk index** — LST (heat hazard) + NDBI (urban sensitivity) + NDVI (cooling capacity) + Population (human exposure)
- **Automatic resampling** — all input rasters are resampled to the LST reference grid without manual preprocessing
- **Strict weight validation** — component weights must sum to exactly 100% (±0.1%), with no silent auto-normalization
- **Flexible reclassification** — each component supports user-defined score breaks; defaults are provided based on literature
- **Manual classification breaks** — optional custom intervals to convert continuous scores to discrete risk classes
- **Dynamic color palette** — classified outputs use a green-to-red gradient that adapts to any number of classes
- **Auto-load in QGIS** — outputs can be automatically loaded and symbolized in the active QGIS project after running
- **Structured output folder** — all results (rasters, vector, report) are written to one organized folder tree
- **Full HTML report** — area statistics, KPI cards, distribution charts, resampling log, and alert flags

---

## Input Parameters

### Raster Layers

| Parameter | Description | Typical Range | Source |
|---|---|---|---|
| **LST Layer** | Land Surface Temperature *(reference grid)* | 20–50 °C | Landsat 8 Band 10/11, MODIS MOD11A1 |
| **NDBI Layer** | Normalized Difference Built-up Index | −1 to +1 | Landsat 8 SWIR1 / NIR |
| **NDVI Layer** | Normalized Difference Vegetation Index | −1 to +1 | Landsat 8 NIR / Red |
| **Population Density** | Human exposure (people/km²) | 0–50,000+ | WorldPop, LandScan, GPWv4, Census |

> **LST is the reference grid.** All other layers are automatically resampled to match LST's extent, resolution, and CRS.

### Reclassification Tables *(optional per component)*

Each component has a default reclassification table. Users may override these by providing custom `Min | Max | Score` break rows. Leave Min or Max blank to denote −∞ / +∞.

### Component Weights

Weights for each of the four components must be provided as percentages summing to exactly **100.0%**.

| Component | Default Weight | Role |
|---|---|---|
| LST | 40% | Primary heat hazard |
| NDBI | 20% | Urban surface sensitivity |
| NDVI | 15% | Cooling / mitigation capacity |
| POP | 25% | Human exposure |

> Recommended ranges: LST 40–60%, NDBI 15–25%, NDVI 15–30%, POP 5–15% *(based on literature)*.

### Manual Classification Breaks *(optional)*

Custom `Min Score | Max Score | Class` intervals for converting the continuous score (1.0–5.0) to discrete integer classes. If left empty, scores are auto-rounded to the nearest integer and clipped to [1–5].

### Open Output After Running

A boolean toggle (default: **on**) that automatically loads and symbolizes all three output layers in the active QGIS project upon completion.

---

## Output Structure

All outputs are written inside **one folder** you select. Sub-folders are created automatically:

```
<output_folder>/
├── rasters/
│   ├── risk_continuous.tif     # Float32 — continuous risk score 1.0–5.0
│   └── risk_classified.tif     # Byte    — integer classes with embedded PAL color table
├── vector/
│   └── risk_classified.gpkg    # Polygon layer with risk attributes
└── report/
    ├── report.html             # Full analysis report with statistics and charts
    ├── high_risk_coords.csv    # Coordinates of highest-risk class pixels
    ├── summary.txt             # Quick statistics summary
    ├── classification_rules.txt# Documentation of applied reclassification rules
    └── resampling_log.txt      # Log of all auto-resampling operations
```

### Vector Attribute Table

| Field | Type | Description |
|---|---|---|
| `risk_class` | Integer | Class value (e.g. 1–5) |
| `risk_label` | String | Label (e.g. Very Low Risk, High Risk) |
| `risk_color` | String | Hex color code matching the raster palette |
| `area_m2` | Double | Polygon area in square meters |
| `area_ha` | Double | Polygon area in hectares |

---

## Risk Classification

Default 5-class scheme (colors adapt dynamically to any number of classes):

| Class | Label | Color | Hex |
|---|---|---|---|
| 1 | Very Low Risk | 🟩 Green | `#008000` |
| 2 | Low Risk | 🟨 Yellow | `#FFDC00` |
| 3 | Moderate Risk | 🟧 Orange | `#FF8C00` |
| 4 | High Risk | 🟥 Red | `#DC1E1E` |
| 5 | Very High Risk | 🔴 Dark Red | `#8B0000` |

> Alert flags are automatically generated in the HTML report when Very High Risk area exceeds 10% or 20% of total coverage.

---

## Component Default Scores

### LST — Land Surface Temperature

| Range (°C) | Score | Description |
|---|---|---|
| < 25 | 1 | Cool |
| 25 – 30 | 2 | Warm |
| 30 – 35 | 3 | Hot |
| 35 – 40 | 4 | Very Hot |
| > 40 | 5 | Extreme |

### NDBI — Normalized Difference Built-up Index

| Range | Score | Description |
|---|---|---|
| < −0.2 | 1 | Natural / non-built |
| −0.2 – 0.0 | 2 | Mixed |
| 0.0 – 0.15 | 3 | Urban |
| 0.15 – 0.3 | 4 | Dense Urban |
| > 0.3 | 5 | Very Dense Built-up |

### NDVI — Normalized Difference Vegetation Index *(inverse)*

| Range | Score | Description |
|---|---|---|
| < 0.0 | 5 | No vegetation — highest risk |
| 0.0 – 0.2 | 4 | Sparse |
| 0.2 – 0.4 | 3 | Moderate |
| 0.4 – 0.7 | 2 | Dense |
| > 0.7 | 1 | Very Dense — lowest risk |

> NDVI classification is **inverse**: higher vegetation = lower heat risk score.

### Population Density *(people/km²)*

| Range | Score | Description |
|---|---|---|
| < 100 | 1 | Rural / sparse |
| 100 – 500 | 2 | Suburban |
| 500 – 1,500 | 3 | Urban |
| 1,500 – 5,000 | 4 | Dense Urban |
| > 5,000 | 5 | Metropolitan Core |

*Thresholds based on UN-Habitat / WHO urban density guidelines.*

---

## Methodology

The urban heat risk index is computed as a **weighted linear combination** of component scores:

```
Risk Score = (w_LST × S_LST) + (w_NDBI × S_NDBI) + (w_NDVI × S_NDVI) + (w_POP × S_POP)
```

Where:
- `S_x` = reclassified score for component x (range 1.0–5.0)
- `w_x` = normalized weight for component x (sum = 1.0)

Each component is reclassified pixel-by-pixel using user-defined or default break intervals before the weighted sum is applied. The resulting continuous score is then discretized into integer risk classes.

**Processing pipeline (8 steps):**

1. Input validation — CRS check, band count, data coverage
2. Auto-resampling — all rasters matched to LST grid via GDAL Warp
3. Array reading — NumPy arrays extracted from resampled GeoTIFFs
4. Reclassification — per-pixel score assignment using break rules
5. Risk computation — weighted summation
6. Classification — continuous score → discrete classes
7. Output writing — rasters (Float32 + Byte PAL) and GeoPackage vector
8. Report generation — HTML report, CSV, summary, and logs

---

## Requirements

- **QGIS** 3.16 or later (LTR recommended)
- **Python** 3.x (bundled with QGIS)
- **GDAL / OGR** (bundled with QGIS)
- **NumPy** (bundled with QGIS)

No additional Python packages required.

---

## Installation

1. Download or clone this repository
2. Copy the plugin folder into your QGIS plugins directory:
   - **Windows:** `C:\Users\<user>\AppData\Roaming\QGIS\QGIS3\profiles\default\python\plugins\`
   - **Linux/macOS:** `~/.local/share/QGIS/QGIS3/profiles/default/python/plugins/`
3. Open QGIS → **Plugins** → **Manage and Install Plugins** → **Installed** → enable **THERMATO**
4. The tool appears under **Processing Toolbox → THERMATO → Urban Heat Risk Analysis**

---

## Authors

Developed by:

- **Prof. Dr. Albertus Deliar, S.T., M.T.** — Institut Teknologi Bandung
- **Prof. Ir. Ketut Wikantika, M.Eng, Ph.D.** — Institut Teknologi Bandung
- **Dr. Alfita Puspa Handayani, S.T., M.T.**
- Hifzhan Zhafir Faza
- M. Titus Gideon
- Prasasta Adhitya Gunawan
- Muhammad Ar Rayyan Ramadhani
- Rafi Dwi Nugroho

Contact: albertus.deliar@gmail.com

© 2026 — Released under the [GNU General Public License v2](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)
