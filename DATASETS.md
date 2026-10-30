# CrisisLens: datasets

Five kinds of data: labelled imagery to train and test the detector, raw imagery for your own event, text reports, context layers, and an independent ground truth to score the ranking against. Everything listed is free for research use, though several sources need registration and licences differ. Record each source and its licence in your datasheet, and keep raw data out of git (see `.gitignore`).

## Choosing your event

Pick **one real flood event** for the end-to-end pipeline before you write any code. A good event has:

- Sentinel-1 passes before and during the flood over your region (check in Earth Engine on day one)
- text reports from the period: ReliefWeb reports, government situation reports or news
- an independent impact source at district or block level: official affected counts, an emergency mapping product, or both
- a manageable area: a handful of districts, not a country

Monsoon floods in Assam, Bihar, Kerala or Bangladesh fit well: Assam's State Disaster Management Authority, for example, publishes district-wise flood reports during the monsoon. Whatever you choose, the event you score the final ranking on must not be one you tuned on.

## Labelled imagery (train and test the detector)

| Dataset | What it is | Use it for |
|---|---|---|
| [Sen1Floods11](https://github.com/cloudtostreet/Sen1Floods11) | Sentinel-1 and Sentinel-2 chips (512×512, 10 m) with hand-labelled flood masks from flood events in several countries; about 14 GB, downloaded with `gsutil` | **Default.** Training and testing flood segmentation; split by event, not by chip |
| [xBD / xView2](https://xview2.org) ([paper](https://arxiv.org/abs/1911.09296)) | Pre- and post-disaster high-resolution imagery with about 850k building polygons labelled by damage level, across many disaster types | The cyclone / earthquake / wildfire track: building-damage assessment |
| [FloodNet](https://arxiv.org/abs/2012.02951) | Drone imagery after Hurricane Harvey with segmentation labels and visual question answering | Very high-resolution flood scenes; VLM experiments (problem 15) |
| [Global Flood Database](https://doi.org/10.1038/s41586-021-03695-w) (Tellman et al., 2021) | MODIS-derived flood extents for hundreds of large floods since 2000, available in Earth Engine | Historical flood frequency as a context layer; coarse sanity checks |

## Raw imagery (your own event)

| Source | What it is | Use it for |
|---|---|---|
| [Sentinel-1 GRD in Earth Engine](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD) | Analysis-ready SAR backscatter, sees through cloud | **Default.** Flood extent during the monsoon; no download needed |
| Sentinel-2 in Earth Engine | Optical imagery, 10 m, blocked by cloud | Water indices when skies clear; visual checks |
| [Copernicus Data Space Ecosystem](https://dataspace.copernicus.eu) | Official Sentinel archive and browser | Downloading scenes outside Earth Engine |
| [Bhuvan](https://bhuvan.nrsc.gov.in) (NRSC/ISRO) | Indian geoportal, including flood-related map layers | Indian context and cross-checking your maps |

Earth Engine is free for noncommercial and research use after registering a Cloud project; do the registration in week 1.

## Text reports

| Source | What it is | Use it for |
|---|---|---|
| [ReliefWeb API](https://apidoc.reliefweb.int) | Humanitarian situation reports, appeals and disaster records | **Default** report stream. Every request needs a pre-approved `appname`; request it on day one |
| Government situation reports | District-wise affected counts, relief camps, damage (e.g. state disaster management authorities) | Extraction target and, kept separate, ground truth |
| [CrisisMMD](https://arxiv.org/abs/1805.00713) and [HumAID](https://arxiv.org/abs/2104.03090) (via [CrisisNLP](https://crisisnlp.qcri.org)) | Labelled disaster tweets (CrisisMMD includes images) | Training and testing classifiers for informativeness and humanitarian categories |
| News articles | Local and national coverage of your event | Extra reports; respect each site's terms |

## Context layers

| Layer | Source | Use it for |
|---|---|---|
| Population | [WorldPop](https://www.worldpop.org), [GHSL](https://ghsl.jrc.ec.europa.eu) | People exposed in flooded areas |
| Buildings | [Google Open Buildings](https://sites.research.google/open-buildings/), [Microsoft Global ML Building Footprints](https://github.com/microsoft/GlobalMLBuildingFootprints) | Structures exposed; building-level damage |
| Rainfall | [GPM IMERG](https://gpm.nasa.gov/data/imerg) (also in Earth Engine) | Rainfall in the days before and during the event |
| Terrain | Copernicus DEM or SRTM (in Earth Engine) | Slope and elevation masks; removing false positives |
| Permanent water | JRC Global Surface Water (in Earth Engine) | Separating floodwater from rivers and lakes |
| Boundaries | [geoBoundaries](https://www.geoboundaries.org), [HDX](https://data.humdata.org) | The spatial units you rank |

## Ground truth for the ranking

| Source | What it is | Caveat |
|---|---|---|
| Government situation reports | Affected population, deaths, houses damaged, by district | Undercounting and revisions; don't also use them as input |
| [Copernicus Emergency Management Service](https://emergency.copernicus.eu) | Expert flood delineation and damage grading for activated events | Only exists for activated events; check yours early |
| [EM-DAT](https://www.emdat.be) | International disaster database, event-level impact | Event-level, not district-level; registration required |
| [GDACS](https://www.gdacs.org) | Global disaster alerts and impact estimates | Model estimates, not observations; a baseline, not truth |

See problem 12 in [PROBLEMS.md](PROBLEMS.md): no single source here is the true answer, so report how your conclusions change across them.
