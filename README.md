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


## References and attribution

1. MathWorks. [*Hurricane Natural Catastrophe Risk Estimation*](https://www.mathworks.com/help/map/hurricane-natural-catastrophe-risk-estimation.html). Original MATLAB tutorial and overall example structure.
2. Bloemendaal, N., et al. (2020). [Generation of a Global Synthetic Tropical Cyclone Hazard Dataset Using STORM](https://doi.org/10.1038/s41597-020-0381-2). *Scientific Data*.
3. Holland, G. (2008). [A Revised Hurricane Pressure–Wind Model](https://doi.org/10.1175/2008MWR2395.1). *Monthly Weather Review*.
4. Emanuel, K. (2011). [Global Warming Effects on U.S. Hurricane Damage](https://doi.org/10.1175/WCAS-D-11-00007.1). *Weather, Climate, and Society*.

## Status

**Work in progress.** This repository currently contains a notebook-based implementation intended for learning, exploration, and further development of hurricane loss modeling in Python.
