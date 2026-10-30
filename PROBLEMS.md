# CrisisLens: open problems

None of these has a settled answer, and each can be probed with a small experiment on one shared pipeline. *Start* is a possible first experiment; *Topics* are things to read up on.

## Seeing damage from above

- **1. Does the model work on a flood it has never seen?** Flood maps from one river basin, season or sensor geometry often degrade on the next. Random train/test splits of image chips hide this, because neighbouring chips from the same event leak into both. How large is the gap between a random split and an event-held-out split? *Start:* train the same model both ways and compare. *Topics:* spatial cross-validation, domain shift, normalisation of SAR backscatter.
- **2. What looks like water but isn't?** SAR flood maps confuse floodwater with permanent rivers, radar shadow from hills, smooth tarmac and wet soil; urban flooding can look *brighter*, not darker. Which errors dominate in your region, and which context layer (permanent water mask, slope, height above nearest drainage) removes most of them cheaply? *Start:* hand-label 100 false positives by cause. *Topics:* SAR backscatter physics, terrain masking, double-bounce in urban areas.
- **3. Is a foundation model worth it on our compute?** A fine-tuned geospatial foundation model may beat a small U-Net, which may barely beat a well-tuned threshold. How much does each step up buy on the held-out event, and at what cost in GPU hours and inference time? *Start:* plot held-out IoU against training cost for threshold, small U-Net and a fine-tuned foundation model. *Topics:* Prithvi, parameter-efficient fine-tuning, cost–accuracy frontiers.
- **4. Damage you can't see from above** (cyclone and earthquake track). In xBD-style data, "minor" and "major" damage are hard to separate even for people, and roof-level views miss walls and interiors. Is the confusion in the model, the labels, or the viewpoint? *Start:* have two of you relabel 200 buildings and compare your agreement with the model's. *Topics:* ordinal classification, label noise, inter-annotator agreement.

## Reading reports

- **5. Where is "near the old bridge"?** Reports name villages, landmarks and roads that are ambiguous, misspelled, transliterated or missing from gazetteers. What fraction of reports can be placed in the right unit, and does an LLM geoparser beat a gazetteer lookup? *Start:* hand-place 100 reports and score both methods. *Topics:* geoparsing, toponym disambiguation, gazetteers (GeoNames, OpenStreetMap).
- **6. Reports lag, repeat and contradict.** The same incident appears in many reports; counts are cumulative in one and daily in another; early numbers get revised. How should duplicates be merged and which number should count? *Start:* trace one district's reported figures over a week. *Topics:* entity resolution, event deduplication, temporal information extraction.
- **7. Reports are not the population.** Text comes from where there is connectivity, journalists and officials. Areas hit hardest can be silent precisely because they are cut off. Does weighting reports bias the ranking toward urban, connected areas? *Start:* compare report density with flooded population per unit. *Topics:* reporting bias, digital divide, missing-not-at-random data.

## Combining signals

- **8. Which modality earns its keep?** Imagery, reports and context each cost effort. Remove one at a time: does the ranking get worse, and for which kinds of units? *Start:* the ablation table from EXPECTATIONS.md, split by urban/rural and reported/unreported units. *Topics:* ablation studies, multimodal fusion (early, late, decision-level).
- **9. What should the score do when a source is missing?** During the monsoon, cloud cover can block optical imagery for days, a SAR pass may not cover the area in time, and some districts send no reports. A missing signal must not read as "no damage". How should the score degrade, and does its uncertainty widen honestly? *Start:* drop each source for a random 30% of units and check whether the ranking and its intervals behave. *Topics:* missing-modality learning, imputation, conservative defaults.
- **10. Transparent formula or learned model?** A weighted index is easy to explain and argue with; a learned model may rank better but is harder to audit. How much accuracy does transparency cost, if any? Can a learned model be explained well enough with feature attributions? *Start:* fit a small gradient-boosted model to the same features and compare rankings and explanations. *Topics:* composite indicators, interpretable models, SHAP.
- **11. How much does the ranking depend on arbitrary weights?** Two reasonable weightings can reorder the top ten. Which units are robustly urgent, and which owe their rank to a modelling choice? *Start:* sample thousands of weight vectors and count how often each unit lands in the top k. *Topics:* sensitivity and uncertainty analysis for composite indicators, rank stability.

## Ground truth and evaluation

- **12. What is the right answer for "priority"?** There is rarely a clean label for where help was most needed. Affected-population counts, damage reports, emergency maps and relief distribution each measure something different, and each has its own biases. Which do you trust, and how much do conclusions change if you swap one for another? *Start:* score the same ranking against two different ground truths. *Topics:* construct validity, proxy labels, measurement error.
- **13. Early and rough versus late and right.** A ranking on day one that's 70% right may be worth more than a perfect one on day five. How quickly does the system reach a useful ranking, and which source arrives first? *Start:* replay the event day by day using only data available by each date. *Topics:* nowcasting, time-to-information, leakage of future data.
- **14. Who does the ranking miss?** Population layers undercount informal settlements; building footprints miss kutcha houses; vulnerability depends on more than headcount. Does the system systematically rank some groups lower than their true need? *Start:* compare ranks for units with similar flooded population but different urbanisation or poverty levels. *Topics:* social vulnerability indices, algorithmic fairness, data gaps.

## Wildcards

- **15. Can a vision–language model just look at the image?** Zero-shot VLMs can describe flooding in an image. Compare one with a trained segmenter on accuracy, cost and failure modes, and check whether its explanations match the pixels.
- **16. From ranking to allocation.** Given a fixed number of boats, teams or relief kits, turn the ranking into an allocation (with travel times from a road network) and measure how many affected people it reaches compared with simple rules.

## If you only take three

Take 1, 8 and 12. They cover whether the imagery holds up on a new event, whether combining sources does anything, and whether you can tell. The rest become side-experiments on the same pipeline.
