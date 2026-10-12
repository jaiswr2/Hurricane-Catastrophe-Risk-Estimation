# Hurricane Catastrophe Risk Estimation in Python

A Python implementation of a hurricane natural-catastrophe (NatCat) risk assessment for a small portfolio of properties in Florida. The notebook combines synthetic tropical cyclone tracks, property locations and values, a wind-field calculation, and a wind-damage function to estimate annual property losses and portfolio-level risk.

This project is adapted from the MathWorks example [Hurricane Natural Catastrophe Risk Estimation](https://www.mathworks.com/help/map/hurricane-natural-catastrophe-risk-estimation.html). It is an independent Python implementation, not an official MathWorks translation or a validated insurance catastrophe model.

## Model overview

The workflow is:

**Synthetic hurricane tracks → Wind speed at each property → Damage ratio → Property loss → Portfolio risk**

The notebook currently:

1. Reads synthetic North Atlantic tropical cyclone data and converts hurricane locations into geographic coordinates.
2. Loads a Florida county-boundary shapefile, identifies hurricane-track points within Florida, and maps the tracks.
3. Geocodes a set of 18 example property addresses in six Florida cities, assigns illustrative property values, and maps successfully located assets.
4. Calculates wind speed at each asset using a Holland-type parametric wind-field formulation implemented in Python.
5. Converts wind speed into a damage percentage and estimates monetary loss for each property.
6. Calculates annual losses over the simulated years, including zero-loss years, using the **maximum wind speed experienced by each property in a year**.
7. Analyzes individual and combined portfolio losses using summary statistics, histograms, spatial loss maps, and a portfolio loss-exceedance/return-period curve.

The damage function is

\[
D(V)=\frac{W^3}{1+W^3},\qquad
W=\frac{\max(V-V_{\mathrm{thresh}},0)}{V_{\mathrm{half}}-V_{\mathrm{thresh}}},
\]

where \(D\) is the **damage fraction**, \(V\) is the property-level wind speed, and the notebook uses \(V_{\mathrm{thresh}}=65\) km/h and \(V_{\mathrm{half}}=253\) km/h. The Python function returns \(100D\) (percentage damage). Monetary loss is therefore calculated as

\[
L_{i,y}=\frac{\mathrm{DamagePercent}_{i,y}}{100}\times \mathrm{AssetValue}_i.
\]

## Repository contents

- [`Rishav-working.ipynb`](Rishav-working.ipynb) — main Jupyter notebook containing data preparation, hazard and exposure visualization, wind and damage calculations, annual loss estimation, and risk analysis.

## Requirements

- Python 3 with Jupyter Notebook or JupyterLab
- `numpy`, `pandas`, `geopandas`, `shapely`, `geopy`, and `matplotlib`
- Internet access for initial address geocoding through Nominatim (unless valid cached coordinates are available)

Install the Python dependencies with:

```bash
pip install notebook numpy pandas geopandas shapely geopy matplotlib
```

## Input data

The notebook currently expects these files in its working directory:

| Input | Purpose |
| --- | --- |
| `STORM_DATA_EC-Earth3P-HR_NA_1000_YEARS_1_IBTRACSDELTA.txt` | Synthetic North Atlantic hurricane tracks covering 1,000 simulated years |
| `FloridaCountyBoundarieswithFDOTDistricts.shp` | Florida county polygons used to construct the area of interest |
| Associated shapefile components (`.shx`, `.dbf`, `.prj`, etc.) | Required to read the shapefile correctly |

The notebook also creates or uses `geocoded_assets.csv` as a local geocoding cache. Input datasets are **not included in the repository as shown here** and must be obtained separately. See the data-source references below; the exact datasets used by the notebook should be documented when distributed.

## How to run

1. Clone or download this repository.
2. Install the dependencies above.
3. Place the hurricane data file and the complete Florida shapefile set in the notebook's working directory, or update their paths in the notebook.
4. Open `Rishav-working.ipynb` in Jupyter Notebook or JupyterLab.
5. Run the cells in order. Review geocoding matches and any reported missing addresses before interpreting portfolio results.

Generated CSV files include `Annual_Property_Losses.csv` and `All_Years_Property_Losses.csv`.

## Outputs and risk measures

- Hurricane-track and property-location maps
- Wind-speed and damage-function plots
- Annual loss estimates for each property and the combined portfolio
- Maximum loss, mean annual loss, and selected annual-loss percentiles
- Annual-loss histograms with logarithmic frequency axes
- Property-level percentile-loss maps
- Empirical portfolio loss-exceedance and return-period plots

Here, **expected annual loss (EAL)** is the mean across *all* simulated years, including years with no modeled loss. A simulated year represents one annual realization, not a chronological future forecast.

## Current limitations

This is a working research/learning example. Results should not be treated as independently validated estimates of insured loss.

- The current hazard filter retains hurricane-center points **inside Florida**; offshore portions of storm tracks are not consistently incorporated in the annual-loss calculation.
- The sample asset values are illustrative, and unsuccessful geocoding may reduce the portfolio below the intended 18 properties.
- The wind model uses projected coordinates (EPSG:3857), and its inland-distance proxy is measured from the Florida state boundary rather than a verified coastline. Wind-speed estimates and units require independent validation.
- Annual damage is based on the largest modeled wind speed in each year; losses from multiple storms in the same year are **not accumulated event by event**.
- In the current notebook, the spatial loss-map calculation uses `quantile(0.9999)` while its plot title says "95th-percentile". Use `quantile(0.95)` if a 95th-percentile map is intended.
- One sample-property-loss cell contains an accidental `88` preceding `florida_assets["DamageProportion"]`; remove it before running the notebook from start to finish.

## References and attribution

1. MathWorks. [*Hurricane Natural Catastrophe Risk Estimation*](https://www.mathworks.com/help/map/hurricane-natural-catastrophe-risk-estimation.html). Original MATLAB tutorial and overall example structure.
2. Bloemendaal, N., et al. (2020). [Generation of a Global Synthetic Tropical Cyclone Hazard Dataset Using STORM](https://doi.org/10.1038/s41597-020-0381-2). *Scientific Data*.
3. Holland, G. (2008). [A Revised Hurricane Pressure–Wind Model](https://doi.org/10.1175/2008MWR2395.1). *Monthly Weather Review*.
4. Emanuel, K. (2011). [Global Warming Effects on U.S. Hurricane Damage](https://doi.org/10.1175/WCAS-D-11-00007.1). *Weather, Climate, and Society*.

## Status

**Work in progress.** This repository currently contains a notebook-based implementation intended for learning, exploration, and further development of hurricane loss modeling in Python.
