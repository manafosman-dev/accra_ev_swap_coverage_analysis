
# Accra EV Battery-Swap Coverage and Site-Selection Analysis

## Overview

This project evaluates the geographical coverage of a hypothetical battery-swap network in Ghana's Greater Accra Region and identifies candidate areas for future stations.

The analysis combines existing-station coverage, developed land, major-road access and nearby demand-related OpenStreetMap features. It is designed as a transparent decision-support workflow rather than a final investment or construction recommendation.

No confidential Kofa Technologies data is used. Existing station locations are treated as portfolio-use inputs.

## Business question

Which developed and road-accessible parts of Greater Accra are outside the existing three-kilometre station coverage, close to potential demand, and sufficiently separated from current and proposed stations?

## Method

1. Load and validate the Greater Accra boundary and existing station points.
2. Project the spatial data into a metre-based coordinate reference system.
3. Create three-kilometre buffers around existing stations and merge overlapping coverage.
4. Identify developed land outside the existing coverage.
5. Restrict the scouting area to land with reasonable access to major roads.
6. Generate a regular grid of possible candidate points.
7. Keep candidates at least three kilometres from existing stations.
8. Keep candidates within 1.5 kilometres of a demand-related location.
9. Count demand-related locations within two kilometres of every remaining candidate.
10. Rank candidates by nearby demand count, using nearest-demand distance as a tie-breaker.
11. Select recommendations while maintaining at least three kilometres between proposed stations.
12. Export static Matplotlib and interactive Folium maps.

## Validated results

| Measure                                                   |        Result |
| --------------------------------------------------------- | ------------: |
| Developed uncovered area                                  |  1,103.76 km² |
| Road-accessible candidate area                            |    776.26 km² |
| Relevant demand locations                                 |         1,238 |
| Initial grid candidates                                   |         3,129 |
| Candidates after the existing-station distance rule       |         3,128 |
| Candidates within 1.5 km of demand                        | 2,439 (78.0%) |
| Minimum spacing in the validated five-site recommendation |      3.162 km |

### Validated five-candidate run

| Rank | Candidate ID | Demand locations within 2 km | Nearest demand (km) | Nearest existing station (km) |
| ---: | -----------: | ---------------------------: | ------------------: | ----------------------------: |
|    1 |          214 |                           72 |               0.472 |                         3.009 |
|    2 |          613 |                           56 |               0.185 |                         3.114 |
|    3 |         1393 |                           46 |               0.064 |                        10.221 |
|    4 |          826 |                           38 |               0.581 |                         3.885 |
|    5 |          411 |                           35 |               0.463 |                         3.011 |

The number of recommendations is configurable. Increasing it does not change the three-kilometre separation rule.

## Key findings

- The analysis identified **1,103.76 km²** of developed land outside the existing three-kilometre coverage.
- The major-road filter retained **776.26 km²**, or approximately **70.3%**, of that developed uncovered area.
- **2,439 of 3,128 valid candidates (78.0%)** were within 1.5 km of a selected demand-related location.
- Demand density varied materially: the median candidate had **9** demand-related locations within two kilometres, while the maximum was **72**.
- The validated five recommendations had **35–72** nearby demand locations and maintained a minimum **3.162 km** separation.
- Candidate 1393 was **10.221 km** from the nearest existing station, making it the strongest geographical extension in the validated five-site set.

These findings support further field investigation, not immediate construction. Exact sites still require land, grid, traffic, cost and operational checks.

## Outputs

Expected project outputs include:

```text
data/processed/recommended_station_candidates.gpkg
data/processed/recommended_station_candidates_named.gpkg
outputs/figures/recommended_station_candidates_named.png
outputs/maps/recommended_stations_simple.html
```

- The GeoPackage preserves the recommendation attributes, geometries and CRS.
- The Matplotlib figure communicates the coverage and suitability analysis.
- The Folium map supports zooming, location inspection and area-name pop-ups.

## Project structure

```text
accra-ev-swap-coverage-analysis/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
├── outputs/
│   ├── figures/
│   └── maps/
├── README.md
├── pyproject.toml
└── uv.lock
```

## Run the project

Install the environment and start JupyterLab:

```bash
uv sync
uv run jupyter lab
```

Run the notebook from top to bottom. Notebook outputs can remain visible after a kernel restart even when their variables are no longer active, so sequential execution is important.

## Main tools

- Python
- GeoPandas and Shapely
- OSMnx and OpenStreetMap
- Matplotlib
- Folium
- pandas and NumPy
- uv and JupyterLab

## Assumptions

- Three kilometres represents an initial straight-line coverage radius.
- Developed land and selected OpenStreetMap features act as proxies for population and demand.
- Road proximity represents initial accessibility, not measured travel time.
- A 1.5-kilometre demand-distance threshold is a transparent screening assumption.
- Demand density is represented by the count of selected demand features within two kilometres.
- Reverse-geocoded community names are approximate labels and require manual verification.

## Limitations

- Straight-line distance does not represent actual travel time or road-network distance.
- OpenStreetMap feature coverage and naming may be incomplete or duplicated.
- The analysis does not include land ownership, parcel availability, electricity capacity, construction costs, traffic volumes, competitor sites or detailed EV adoption data.
- Candidate points identify areas for investigation, not confirmed station addresses.
- Final decisions require field surveys and operational, commercial and regulatory assessment.

## Data attribution

Boundary, road, developed-land and demand-related features are derived from [OpenStreetMap](https://www.openstreetmap.org/) contributors.

## Repository

[accra_ev_swap_coverage_analysis](https://github.com/manafosman-dev/accra_ev_swap_coverage_analysis)
