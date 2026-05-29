```html
<div style="display: flex; align-items: center;">
  <p align="center">
    <img src="/Thermato/icon/thermato.png" alt="THERMATO Logo" style="width: 250px; height: 250px;">
    <h1>THERMATO (Thermal Hazard Monitoring and Assessment Tool)</h1>
  </p>
</div>

---

**THERMATO** is a QGIS plugin designed to assess urban heat risk by integrating Land Surface Temperature (LST), Built-up Density (NDBI), Vegetation Density (NDVI), and Population Density into a unified spatial risk model. The plugin provides automated raster harmonization, weighted risk modeling, classification, vectorization, and reporting tools to support climate adaptation planning, environmental assessment, and urban resilience studies.

---

## 🔧 List of Tools

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Heat Risk Analysis" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Heat Risk Analysis</h2>
</div>

Combines Land Surface Temperature, Built-up Density, Vegetation Density, and Population Density into a single urban heat risk index using a weighted multi-criteria approach.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Automatic Raster Harmonization" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Automatic Raster Harmonization</h2>
</div>

Automatically aligns all input rasters to a common reference grid by matching spatial resolution, extent, and coordinate system, eliminating manual preprocessing requirements.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Flexible Reclassification" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Flexible Reclassification</h2>
</div>

Allows users to customize classification thresholds and scoring schemes for each indicator while providing literature-based default classifications.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Weighted Risk Modeling" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Weighted Risk Modeling</h2>
</div>

Calculates urban heat risk through a weighted scoring framework with strict validation to ensure reproducible and transparent analysis.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Dynamic Risk Classification" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Dynamic Risk Classification</h2>
</div>

Converts continuous risk scores into classified heat risk zones using automatic or user-defined class intervals with adaptive color palettes.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Vectorization and Area Statistics" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Vectorization and Area Statistics</h2>
</div>

Automatically transforms classified raster outputs into vector polygons and calculates area statistics for each heat risk category.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="Automated Reporting" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>Automated Reporting</h2>
</div>

Generates comprehensive HTML reports containing risk statistics, class distributions, classification rules, processing logs, and analytical summaries.

---

<div style="display: flex; align-items: center;">
  <img src="/Thermato/icon/thermato.png" alt="QGIS Integration" style="width: 100px; height: 100px; margin-right: 20px;">
  <h2>QGIS Integration</h2>
</div>

Automatically loads generated raster and vector outputs into the active QGIS project with predefined visualization settings and classifications.

---

## 📦 Output Types

- GeoTIFF: Continuous Heat Risk Raster
- GeoTIFF: Classified Heat Risk Raster
- GeoPackage: Classified Risk Polygons
- HTML Report: Automated Risk Assessment Report
- CSV: High-Risk Area Coordinates
- TXT: Classification Rules Documentation
- TXT: Processing and Resampling Logs
- Area Statistics and Risk Distribution Summaries

---

## 📌 Recommended Use

THERMATO is best suited for urban planners, environmental researchers, climate scientists, GIS analysts, and policy makers requiring spatial insights into urban heat vulnerability, climate adaptation, and sustainable city development.

---

## 🧾 License

This plugin is licensed under the GNU General Public License v2 (or later). See the LICENSE file for details.
```
