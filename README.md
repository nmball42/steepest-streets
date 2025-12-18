# Steepest streets in Millbrae using OpenStreetMap and OSMnx

Last updated: Dec 18th 2025

OpenStreetMap and the OSMnx add-on for viewing streets as a network can be used to find the steepest streets in a neighborhood. Loosely based on their tutorial, we find the steepest streets in my home San Francisco Bay Area suburb of Millbrae, make a map of street grades that turned out to be useful for walking the area, and show views looking up the streets via the Google Street View API. The same code works on any named area present in the well-known Nominatim database.

## Requirements

- Machine that can run Conda virtualenv and Jupyter notebook, e.g.,
  - Visual Studio code with
    - Python extension
    - Jupyter extension
    - Install iPyKernel
- Google Street View API key in GCP project that has billing

## Setup

- Create virtualenv, e.g., `.conda` in current directory in VSCode
- `conda install pip`
- `pip install osmnx[all]`
- Add your Google Street View API key to `steepest_streets.ipynb` to code cell in settings section

## Run

- Run Jupyter notebook `steepest_streets.ipynb`
- Open the URL output by the last cell to see the steepest street in Google Street View

The place query can be changed to any name valid in the Nominatim database.

Remove the API key if storing file anywhere public.

## Improvements

- Manual colormap by absolute gradient, e.g., 0:0.05:0.25+ grade (narrow bins can also directly highlight steepest grades for a given city)
- Refine the analysis to be more robust in places such as San Francisco
  - Filter out any segments of obviously spurious grade, say over 40%, and too-short length, say less than 10 meters
  - Manually inspect remaining Street View images
  - Manually adjust street node points so they correspond to the satellite image of the intersection (being sure that the network and raster are precisely aligned using a local CRS projection)
  - Remove the OSMnx simple intersection default
- Minimum threshold length on street segments, e.g., 10m
- Make a webapp so users can see their neighborhood without writing code
- Generate server-side digital signatures for currently unsigned Street View Static API requests
