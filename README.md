# Orthometric Height Calculator for GCP Feature Services

A small Jupyter notebook that fills in missing orthometric heights on a
Ground Control Point (GCP) feature service hosted on ArcGIS Online or
ArcGIS Enterprise, using NOAA's public geoid height API.

For each GCP missing an orthometric height, the notebook:

1. Queries the [NOAA NGS Geoid Height API](https://geodesy.noaa.gov/GEOID/GEOID18/computation.html)
   for the geoid separation at that point's latitude/longitude.
2. Calculates orthometric height as `ellipsoidal (GNSS) altitude − geoid separation`.
3. Writes the geoid separation and orthometric height back to the feature
   service.
4. Saves a dated CSV snapshot of the results locally.

## Requirements

- Python 3.9+
- [`arcgis`](https://developers.arcgis.com/python/) (ArcGIS API for Python)
- `pandas`
- `requests`

```bash
pip install arcgis pandas requests
```

## Setup

1. Open `orthometric_height_gcp.ipynb`.
2. In the **Configuration** cell, set `GCP_ITEM_ID` to your feature layer's
   ArcGIS Online/Enterprise item ID, and adjust the field names if your
   schema differs from the defaults.
3. In the **Connect to ArcGIS Online** cell, authenticate. `GIS("home")`
   works automatically inside ArcGIS Online Notebooks or ArcGIS Pro's
   notebook environment. Elsewhere, connect explicitly:

   ```python
   gis = GIS("https://www.arcgis.com", "your_username")
   # or
   gis = GIS(profile="your_saved_profile")
   ```

4. Run all cells.

## Notes

- Only GCPs with a null orthometric height field are processed — running
  the notebook again is safe and will simply find nothing left to update.
- Edits are written back to the feature service via `edit_features`, so make
  sure the connected account has edit privileges on the layer.
- The CSV log is written to `./output/` by default (configurable via
  `OUTPUT_DIR`).

## License

Released under the [MIT License](LICENSE).
