<img width="1209" height="860" alt="image" src="https://github.com/user-attachments/assets/3666eddc-ae3b-44eb-b7d3-451cbdfd1319" />

# CrisisLens

Multimodal disaster intelligence. A system that combines satellite imagery, incident reports and environmental data to estimate where damage is and decide where help should go first, plus an evaluation that shows whether combining the signals actually ranks areas better than any one of them alone.

The research question is whether fusing sources produces a better, explainable response priority, not whether a bigger model draws sharper flood maps.

**Status:** planning. The repo holds the project brief and study material; there is no code yet.

## What we're building

1. **The detector.** Maps flood extent (or building damage) from satellite imagery, starting from a simple threshold baseline and moving to a learned model.
2. **The extractor.** Pulls location, impact type, severity and time out of text reports, and places each report on the map.
3. **The prioritizer.** Combines imagery, reports and context (population, buildings, rainfall, terrain) into a per-area priority score where every number can be traced back to its inputs.
4. **The findings.** Which sources earn their place, how the system does on a disaster it has never seen, and how its ranking compares with what actually happened on the ground.

Scope: about 7–8 weeks, intermediate to advanced. Start with **floods** and one region; extend only once the full pipeline works. Hackathon adaptations: floods, cyclones, wildfires, earthquakes, emergency resource allocation.

**Compute:** everything here is designed to run on a laptop, Google Colab and Google Earth Engine. No step needs more than one free-tier GPU. If something does, scope it down.

## In this repo

| File | What it is |
|---|---|
| [EXPECTATIONS.md](EXPECTATIONS.md) | What the final product must be and do, and when it counts as done |
| [PROBLEMS.md](PROBLEMS.md) | Sixteen open-ended problems to investigate |
| [STUDY.md](STUDY.md) | Ten topics to study, each with a question to answer and links to the readings |
| [DATASETS.md](DATASETS.md) | Labelled datasets, raw imagery, text sources, context layers and ground truth, with what each is good for |

Start with EXPECTATIONS.md, pick a region and event from DATASETS.md, pick problems from PROBLEMS.md, and use STUDY.md alongside as you build.

## Licence

Documentation in this repository is released under the [Apache License 2.0](LICENSE). Datasets referenced here have their own licences; see [DATASETS.md](DATASETS.md).
