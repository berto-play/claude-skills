# Analytical Design — *Envisioning Information*, *Visual Explanations*, *Beautiful Evidence*

Extensions beyond the first book. Load when designing dense, multivariate, or explanatory displays.

---

## The six principles of analytical design

From *Beautiful Evidence*. These govern any display meant to support reasoning about evidence.

1. **Show comparisons, contrasts, differences.** The core question is always *"compared to what?"* A number or trend means nothing without its reference. Design the comparison in, don't leave the reader to supply it.
2. **Show causality, mechanism, structure, explanation.** Good analytical graphics do more than describe — they help the reader see *why*. Where a mechanism exists (a threshold crossed, a dependency failing), show it.
3. **Show multivariate data** — more than one or two variables. The world is multivariate; reducing it to a single series to make drawing easy discards the structure that matters.
4. **Completely integrate evidence** — words, numbers, images, diagrams in the same visual field. Do not segregate the chart from the legend from the caption from the table. The reader should not have to reassemble the argument.
5. **Document thoroughly.** Titles, scales, sources, units, authors, dates. A graphic making a claim must show where its numbers came from.
6. **Content quality is decisive.** No amount of technique rescues thin or wrong content. "Analytical presentations ultimately stand or fall depending on the quality, relevance, and integrity of their content."

Reduced to a maxim: **the principles of analytical design are the principles of good reasoning, applied to seeing.**

---

## Sparklines

A **small, intense, word-sized graphic** embedded inline with text, numbers, or in a table row. "Datawords."

- Height ≈ the line height of surrounding text; width a few dozen to a few hundred pixels.
- No axes, no gridlines, no labels inside the sparkline itself. Context comes from its neighbors — a label to the left, the current value direct-labeled to the right.
- Optional: a light band for the normal range, a marker dot for the latest point, red/emphasis for a breach.
- **Best use:** one row per entity in a table — name, current value, and the sparkline showing how it got there. Hundreds of rows resolve at a glance and sort by whatever matters. This is the backbone of a truthful status board.

---

## Layering and separation

The eye must be able to pull the primary data forward and let context recede.

- **Value contrast does the layering.** Primary data: dark, saturated, or heavier stroke. Context (baselines, grids, medians, envelopes): light gray, thin, receding.
- **1 + 1 = 3 or more.** Two elements create additional visual noise between them (implied edges, tension). Minimize the interactions the reader has to ignore.
- Spend color narrowly — one accent for the one thing that must be seen (the anomaly, the selection). Everything else in grayscale so the accent actually reads.

---

## Micro / macro reading

A great display works at two distances at once:
- **Macro:** step back, and overall pattern and shape appear (the fleet is healthy; the trend is up).
- **Micro:** lean in, and individual data points, labels, and detail are all still there to be read.

Do not sacrifice the micro to summarize the macro. Detail *supports* the overview; it is not clutter. A dense, well-ordered display gives both; a summary tile gives only a thin macro and throws the micro away.

---

## Range-frames and dot-dash plots

- **Range-frame:** draw each axis line only across the actual span of the data, not the full plot rectangle. The frame now also tells you the min and max of each variable — the axis does double duty.
- **Dot-dash plot:** add tick marks along the axes at each data point's position (a marginal rug), so the axes also show the marginal distribution of each variable. More information, same ink.

---

## Causality, confections, and mechanism

- **Show cause where you can.** A chart that shows an effect beside its cause (deployment marker on a latency spike; policy change on a trend) explains, not just reports.
- **Confections** — assemblages of disparate images/panels arranged to make an argument. Legitimate for explanation, but every element must earn its place; a confection with decorative filler is just chartjunk with a thesis.
- Prefer showing the **mechanism** over asserting the conclusion in a title. Let the reader see it.
