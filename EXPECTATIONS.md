# CrisisLens: what the final product must be and do

**In one sentence:** a pipeline that takes satellite imagery, text reports and context data for a disaster, estimates where the damage is, ranks areas by how urgently they need response, explains each ranking, and is tested against what really happened on a disaster it was not built on.

The findings matter more than the dashboard. The research question is whether combining sources gives a better and more trustworthy priority ranking than imagery, reports or context alone, and what each source contributes.

"Better" means four things, reported side by side rather than as one score (a model that ranks every populous district first must not look good):

- the damage estimate is accurate: pixels or buildings, scored on events the model never trained on
- the priority ranking agrees with ground truth on where impact was worst
- every score can be explained: which inputs drove it, and how confident the system is
- you know what it costs and how fast it is: compute, data access, and how many hours after the event the first useful ranking appears

## What you hand in

1. **The pipeline**, runnable end to end on one real event of your choice (a monsoon flood in India or Bangladesh by default; see DATASETS.md).
2. **The evaluation set**: the held-out event or events, the ground truth you scored against, and the scoring code.
3. **The findings**: a write-up that answers the comparisons below with numbers.

## The pipeline must

- Produce a flood-extent map (or building-damage map) from imagery. Include a non-learned baseline (e.g. Sentinel-1 change detection with a threshold) and at least one learned model.
- Extract structured records from text reports: where, what kind of impact, how severe, when. Link each record to an administrative unit or grid cell, and keep the source sentence.
- Aggregate everything to a common spatial unit (district, block or grid cell) with exposure layers: population, buildings and at least one environmental variable (rainfall, elevation or river proximity).
- Compute a priority score for each unit that decomposes into named contributions, so a reader can see *why* one area ranks above another.
- Attach uncertainty to each score, and flag units where a source is missing (cloud cover, no reports) instead of silently scoring them as safe.
- Treat each modality as an on/off switch, so every ablation runs from config.
- Log every run (settings, inputs used, per-unit scores and contributions, runtime) so results can be rebuilt without reprocessing imagery.

## The evaluation must

- Score imagery models on a **held-out event**, never on random chips from the training events. Report IoU and F1 for flood maps, or per-class F1 for building damage.
- Score text extraction on a hand-labelled sample: precision and recall per field, plus how often the location lands in the correct unit.
- Score the ranking against an independent ground truth (official affected counts, damage reports or emergency mapping products): rank correlation, top-k overlap, and a "how many affected people would the top-k units reach" curve.
- Include simple baselines for the ranking: population alone, imagery alone, reports alone.
- Never score the system using the same reports or maps it took as input.
- Be documented well enough to rebuild: which event, which dates, which sources, which licences.

## The findings must

- Show what each modality adds by removing one at a time, broken down by unit type (urban versus rural, many reports versus none).
- Compare the threshold baseline with the learned imagery model, and say whether the learned model's gain survives on the unseen event.
- Compare a transparent weighted score with at least one learned or data-driven alternative, and say what transparency costs.
- Show how the ranking changes as weights change: which units are stable at the top, and which depend on arbitrary choices.
- Put an uncertainty range on every comparison, and say "no detectable difference" when that's what the data shows.
- Show where the best system still fails, with maps and examples.
- Investigate at least two problems from PROBLEMS.md and report what you found, null results included.

## Should have, if time allows

- A map interface (Streamlit, a notebook with folium/leafmap, or a static web page): the priority map, a click-through breakdown of any unit's score, and the source reports and imagery behind it. The dashboard in the card's thumbnail is the kind of thing meant.
- A second event or a second disaster type (e.g. cyclone damage with xBD), to check whether the findings transfer.
- A timeline view: how the ranking evolved as imagery and reports arrived over the first days.
- A short demo video and slides.

## Not expected

Covering every disaster type, real-time operational deployment, training a geospatial foundation model from scratch, buying commercial high-resolution imagery, scraping platforms whose terms forbid it.

## Done when

- [ ] Someone outside the team can run one command and rebuild the headline results table and priority map from cached intermediate files.
- [ ] A live demo shows a flood map for the held-out event, a report being placed on the map, and one unit's priority explained input by input.
- [ ] The write-up answers each comparison above in plain words, with a limitations section that says who the ranking might miss and why.
