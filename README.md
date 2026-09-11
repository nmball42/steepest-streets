# Steepest streets in Millbrae using OpenStreetMap and OSMnx

Last updated: Feb 06th 2026

OpenStreetMap and the OSMnx add-on for viewing streets as a network can be used to find the steepest streets in a neighborhood. Loosely based on their tutorial `12-node-elevations-edge-grades.ipynb`, we find the steepest streets in my home San Francisco Bay Area suburb of Millbrae, make a map of street grades that turned out to be useful for walking the area, and show views looking up the streets via the Google Street View API. The same code works in principle on any named area present in the well-known OpenStreetMap Nominatim geocoding tool. ([Blogpost](https://nickballdatascience.com/finding-the-steepest-streets-in-millbrae/))

## Disclaimer
This is a personal project built as part of my transition from generalist data scientist to specializing in geospatial data science, GIS, and GeoAI. The aim is for this project to be shareable, but it is not designed for production use.

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

To existing features

- Manual colormap by absolute gradient, e.g., 0:0.05:0.25+ grade (narrow bins can also directly highlight steepest grades for a given city), instead of gradient quantiles, so flat areas appear mostly low-gradient colors
- Minimum threshold length on street segments, e.g., 10m
- Test on Nominatim areas defined by a radius instead of name, e.g., "Sheffield, England" does not work well by name
- Plot top 10 steepest streets over basemap and label with numbers
- Add an option to use Open Topo data instead of Google elevation data so that the code can optionally be run without requiring the user to set up a Google API key
- Refine the analysis to be more robust in places such as San Francisco. From `https://github.com/gboeing/osmnx-examples/blob/main/notebooks/12-node-elevations-edge-grades.ipynb`: "Note that there is some spatial inaccuracy in elevation data resolution. For example, in San Francisco (where Google's resolution is ~ 19 meters) a couple of edges in hilly parks have a 50+ percent grade because Google assigns one of their nodes the elevation of a hill adjacent to the street."
  - Filter out any segments of obviously spurious grade, say over 40%, and too-short length, say less than 10 meters
  - Manually inspect remaining Street View images
  - Manually adjust street node points so they correspond to the satellite image of the intersection (being sure that the network and raster are precisely aligned using a local CRS projection)
  - Remove the OSMnx default consolidation of intersections, which may over-simplify some short steep segments
- Option to output results for a place to .gpkg or other format
- Resolve street segment into its individual line segments for sufficiently curved streets that may misalign to Street View direction that points to the node at the other end of the street segment, and point Street View along the line segment
- Generate server-side digital signatures for currently unsigned Street View Static API requests
- Get the interactive Street View using the Javascript API instead of the static API
  
## Extensions

Add new features

- Share to targeted audience, e.g., OSM Diaries
- Make a webapp so users can see their neighborhood without running .ipynb, installing software, or writing code
- Python .toml to enable pip install and run this project, with prompt & howto for API key
- Street that needs most power to climb: combine segments with same name, average gradient * distance
