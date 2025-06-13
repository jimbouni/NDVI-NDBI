# Guyana Land-Cover Change (2020–2025)

**Assessing vegetation loss and built‑up expansion in Guyana’s oil‑boom era with Sentinel‑2 and Google Earth Engine**

---

## Overview

This repository contains a Google Earth Engine (GEE) script that:

1. Builds annual median composites of Sentinel‑2 surface‑reflectance imagery for **2020** and **2025**.
2. Calculates **NDVI** (vegetation) and **NDBI** (built‑up) indices for each year.
3. Derives ΔNDVI and ΔNDBI rasters (*2025 minus 2020*).
4. Generates national summary statistics and per‑region zonal statistics.
5. Exports rasters and tables to Google Drive.
6. Presents quick‑look visual layers and an interactive legend inside the GEE Code Editor.

> **Research context** — The outputs feed Chapter 4 of my MSc dissertation, testing whether Guyana’s recent petroleum windfall coincides with spatially uneven land‑cover change.

---

## Repository structure

```
.
├── gee_script.js        # Main GEE code (mirrors the script in this README)
├── docs/
│   ├── screenshots/     # Optional: PNGs/GIFs for the GitHub README
│   └── flowchart.pdf    # High‑res methodology flow‑chart for the thesis
├── data/                # Empty placeholder — large rasters are kept in GEE / Drive
└── LICENSE
```

---

## Prerequisites

* A **Google Earth Engine** account
  Sign up at [https://earthengine.google.com/](https://earthengine.google.com/).
* **Google Drive** with ≥ 5 GB free space (for exported GeoTIFFs/CSVs).
* Web browser that supports the GEE Code Editor (Chrome/Firefox recommended).

### Optional local tools

* `geeadd` or `earthengine` Python API for batch asset uploads (only if you plan to fork and scale‑out the workflow).
* QGIS / ArcGIS Pro for post‑processing.

---

## Quick‑start

1. **Clone this repo**

   ```bash
   git clone https://github.com/<your‑user>/guyana‑landcover‑change.git
   cd guyana‑landcover‑change
   ```
2. **Open `gee_script.js` in the GEE Code Editor**
   *Click “New → Script” → paste code → Save.*
3. **Add the vector boundary asset**
   Upload your `gy` shapefile or change the asset ID in the script (line 9).
4. **Run the script**
   *Runtime ≈ 6–10 minutes (depends on Google servers).*
   The map pane will display NDVI/NDBI layers; Drive exports will appear in the **Tasks** tab once complete.

---

## Detailed workflow

| Step | Function                 | Key parameters                    | Output                                                    |
| ---- | ------------------------ | --------------------------------- | --------------------------------------------------------- |
| 1    | `buildComposite()`       | `year ∈ {2020, 2025}`             | Annual median SR composite clipped to Guyana              |
| 2    | `addNDVI()` `addNDBI()`  | Bands: B8, B4, B11                | NDVI/NDBI bands appended                                  |
| 3    | Δ calculation            | Raster subtraction                | `ΔNDVI_2020_2025`, `ΔNDBI_2020_2025`                      |
| 4    | `reduceRegion()`         | Mean, Min, Max, SD                | National summary table (`Change_Stats_2020_2025.csv`)     |
| 5    | `reduceRegions()`        | Same reducers                     | Zonal stats per admin region (`ZonalStats_2020_2025.csv`) |
| 6    | `Export.image.toDrive()` | `scale = 10 m`, `crs = EPSG:4326` | GeoTIFFs for Δ rasters                                    |
| 7    | Interactive legend       | `ui.Panel` widgets                | Colour‑ramp guide in GEE GUI                              |

---

## Customisation

* **Cloud threshold** — change `MAX_CLOUD_PROB` (default = 65).
* **AOI** — swap the `guyana` geometry for another FeatureCollection.
* **Years** — call `buildComposite(<year>)` for additional periods.
* **Indices** — plug in other band maths (e.g., NDWI) by copying the pattern in `addNDVI()`.

---

## Expected outputs

| File                                | Description                                            |
| ----------------------------------- | ------------------------------------------------------ |
| `Delta_NDVI_2020_2025_Guyana.tif`   | 10‑m GeoTIFF of vegetation change                      |
| `Delta_NDBI_2020_2025_Guyana.tif`   | 10‑m GeoTIFF of built‑up change                        |
| `Guyana_Change_Stats_2020_2025.csv` | One‑row CSV: national mean/min/max/SD for both indices |
| `ZonalStats_2020_2025.csv`          | Per‑region stats (admin boundaries in `gy`)            |

> **Note** — Large rasters are *not* version‑controlled here. They live in your Drive bucket to keep the repo lightweight.

---

## Citation

If you use or adapt this code, please cite:

> Nedd, J. (2024). *Guyana’s Oil Boom and Spatial Inequality: A Remote‑Sensing Assessment* \[MSc dissertation, University X].

---

## License

[MIT](LICENSE) — free to re‑use, but please give credit.
