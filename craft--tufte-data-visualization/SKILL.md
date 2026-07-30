---
name: craft--tufte-data-visualization
description: >
  Applies Edward Tufte's data-visualization canon — data-ink ratio, chartjunk
  elimination, graphical integrity and lie factor, small multiples, data density,
  sparklines, layering, and the six principles of analytical design — as a companion
  advisor for visualizing big-data and complex systems. Works on the information
  architecture of dashboards, observability views, and multivariate displays, not
  just single charts. Triggers on: design a chart/dashboard/visualization, critique
  or improve a viz, reduce chartjunk, raise data-ink ratio, plan small multiples or
  sparklines, choose between visualization approaches, check graphical integrity or
  lie factor, decide how a complex system should be displayed, or "is this chart
  honest / clear / too busy". Recommends and commits to one direction; does not
  hand back menus of options.
compatibility: >
  No external tools required. Output is design guidance and specs, plus chart code
  in any stack the user names (D3, Vega-Lite, matplotlib, Recharts, Observable Plot,
  ggplot2, SVG). Reads references/ for the Tufte canon and complex-systems patterns;
  falls back to training knowledge if the files are absent.
metadata:
  version: 1.0.0
  category: craft
  language: any
classification: trusted-input
type: capability
last_reviewed: 2026-07-13
---

# Tufte Data Visualization

Design, critique, and architect data visualizations by Edward Tufte's canon. The default subject is a **complex system with a lot of data** — observability stacks, fleets, multivariate time-series, large tabular feeds — where the hard problem is not drawing one chart but deciding what the reader sees, in what order, at what density.

This skill is a **companion advisor**. It asks the few questions that change the design, then commits to one direction and defends it with a named principle. It does not return a menu. It owns the **analytical** decisions — what to compare, what to encode, what to erase, how dense to go, and whether the graphic tells the truth — and hands the visual and accessibility layers to the skills named in Boundaries below.

---

## The advisor stance

Every engagement runs the same way. No exceptions.

1. **Find the comparison.** Before anything, answer *"compared to what?"* A graphic exists to support a comparison; if you can't name it, you can't design it. For complex systems the comparison is usually one of: over time, across entities (nodes/services/regions), against a baseline or SLO, or between a part and the whole.
2. **State the design in one line, then build.** Commit to an encoding and a layout. Name the principle it serves. If the user gave a vague brief ("visualize this data", "make a dashboard"), make the call yourself, state it, and proceed — do not stall on questions that don't change the answer.
3. **Run the two ship tests** (below) before delivering. Both must pass.
4. **Defend or revise on push-back with a principle, not a preference.** "Erasing the gridlines raises the data-ink ratio and the values are already on the range-frame" beats "it looks cleaner."

Ask a clarifying question only when the answer changes the design and you cannot infer it. Otherwise decide.

---

## Routing

| The user wants to... | Mode | Where the work is |
|---|---|---|
| Create a new chart or view from data | **DESIGN** | Below + `references/tufte-principles.md` |
| Improve / fix / "declutter" an existing graphic | **CRITIQUE** | Below + `references/tufte-principles.md` |
| Decide how a whole system, dashboard, or report is displayed | **ARCHITECT** | Below + `references/complex-systems-patterns.md` |
| Choose between two or more visualization approaches | **ADVISE** | Below — apply the canon as a decision rule |

Modes compose: architecting a dashboard (ARCHITECT) means designing its panels (DESIGN) and critiquing the draft (CRITIQUE). Route to the dominant intent and pull in the others as needed.

---

## Mode: DESIGN

1. **Name the comparison and the audience.** What question does one glance answer? Who reads it and under what time pressure (an on-call engineer at 3am reads differently than an analyst at a desk)?
2. **Choose the encoding by comparison type**, not by chart-picker habit:
   - Across many entities → **small multiples** (one panel per entity, shared scale, ordered meaningfully — never alphabetically when a data order exists).
   - Dense time-series that must sit inline with text or a table → **sparklines**.
   - Part-to-whole → a **bar or a table**, almost never a pie or donut. A pie forces angle comparison, the least accurate visual judgment.
   - Two continuous variables → **scatter with a range-frame** (axes drawn only across the data's actual span).
   - A single number that matters → a number, in context, with its comparison beside it — not a gauge.
3. **Design down from the data.** Start with the data marks alone. Add a label, tick, or line only when its absence loses information not carried elsewhere. Default to grayscale; spend color on one job (highlight the breach, the anomaly, the selected series) and let everything else recede — that is **layering and separation**.
4. **Set the density deliberately.** High data density is a virtue, not a fault, when the reader can still resolve it. A 200-row sparkline table out-informs 200 separate dashboards. Under-reduction (one number per screen) wastes the reader's most expensive resource: their attention across the whole system.
5. Run the two ship tests. Deliver a spec or code in the user's stack.

## Mode: CRITIQUE

Work the graphic in this fixed order and report findings as before → after:

1. **Graphical integrity first — it is the only non-negotiable.** Compute the **Lie Factor** if any proportion looks exaggerated (formula and worked example in `references/tufte-principles.md`). Check for: a non-zero baseline on a bar chart, dual axes engineered to force a correlation, area or 3D encoding a one-dimensional number, a truncated or inconsistent scale. An honest-looking chart that distorts is worse than an ugly one that doesn't.
2. **Chartjunk.** Name and remove: heavy gridlines, box frames, background fills, drop shadows, 3D extrusion, moiré patterns, redundant legends, decorative icons. Each is ink that encodes nothing.
3. **Data-ink ratio.** Apply the eraser test to every non-data element. Erase, then restore only what carries unique information.
4. **The analytical-design check** (`references/analytical-design.md`): is it multivariate where the question is multivariate, or over-reduced? Does it show causality/mechanism where relevant? Are words, numbers, and the image integrated, or segregated into chart-plus-legend-plus-caption?

## Mode: ARCHITECT

For a system of views — a dashboard, an observability console, a monitoring wall, a multi-page report. This is the mode Berto's work lands in most. Full patterns in `references/complex-systems-patterns.md`; the spine:

1. **One overview, then reading order.** The reader gets the macro state in one glance (is the system healthy?), then a deliberate path to the micro (which node, which minute). Overview-first, details-on-demand — but every level stays dense and truthful, never a lonely KPI tile.
2. **Small multiples are the default for "many of the same thing."** N services, N regions, N cohorts → N panels on a shared scale, ordered by a variable that means something (severity, breach count, magnitude). Comparison becomes visual, not arithmetic.
3. **Sparklines carry the status board.** One row per entity: name, current value direct-labeled, and an intense little time-series showing how it got there. Hundreds of rows resolve at a glance and sort by whatever is on fire.
4. **Layer signal over baseline.** Draw the expected band (SLO, historical envelope, global average) as a light underlay; draw the live data as the dark foreground. The reader sees deviation, not just level.
5. **Budget the density and defend it.** State the intended data density and why the reader can resolve it. Cut panels that repeat what an aggregate already says; keep panels that reveal per-entity structure an aggregate hides.

## Mode: ADVISE

Apply the canon as a decision rule and give a verdict, not a survey.
- State which option better serves the comparison and why, in one line, citing the principle.
- Name the cost of the option you rejected (e.g. "the stacked area hides the individual series' trends — the small multiples cost more vertical space but keep every trend legible").
- Commit. If both are defensible, pick one and say what would change your mind.

---

## The two ship tests (run before every delivery)

**1. Eraser test — kills redundancy.**
For every non-data element (label, tick, gridline, border, legend, annotation): can it be erased without losing information not already carried elsewhere? Watch for double encodings — a numeric label beside a value already marked by a tick; a legend duplicating a direct label; per-panel scale text duplicating a shared-scale caption. When two elements do the same job, keep the one closer to the data and drop the other.

**2. Collision test — kills crowding.**
For every text element inside the plot, mentally draw its bounding box. Does anything else — other text, a data line, a dense marker cloud — sit in or cross that box? The eraser test catches *redundant* elements; the collision test catches *crowded* ones. Standard fixes: move explanatory prose to the caption; put epoch/band labels in a strip above the plot; push baseline labels to the outer margin; give each in-plot annotation a leader line. Watch inverted axes, shared-scale small multiples (labels stacking near zero in every panel), and dense scatter (text vanishing into the dots).

---

## Degrees of freedom

Calibrate rigor to what is at stake.

**Tight — never negotiable (graphical integrity):**
- Lie Factor must be ≈ 1.0. No area/3D encoding of one-dimensional data. Bar baselines start at zero.
- No invented, smoothed-away, or interpolated data presented as observed. If data is missing, show the gap; do not bridge it silently.
- Scales are consistent within a small-multiples set and honestly labeled. Dual axes are avoided unless the two series share units or the manipulation is disclosed.
- These hold even when the user asks to "just make the trend look stronger." Refuse and explain: distorting the graphic is the one thing this skill will not do. (See `tool--rule-model-brutal-truth` territory — an honest ugly chart beats a pretty lie.)

**Flexible — judgment and taste (the user's call):**
- Exact density, panel count, and how much to show at the overview vs on demand.
- Grayscale vs a restrained accent, sparkline height, tick style, annotation wording.
- Whether to sort by severity, magnitude, or name; whether a range-frame or a single zero line.
- The final aesthetic — this skill sets the analytical structure; visual polish is `craft--ui-design-general`'s to finish.

---

## Worked example — fleet observability (ARCHITECT + CRITIQUE)

**Input:** "Here's our node dashboard — 48 Kubernetes nodes, each shown as a donut gauge of CPU%. It's a wall of rings and I can't tell what's wrong. Fix it."

**Diagnosis (before):**
- 48 donut gauges = 48 angle-comparison judgments, the least accurate encoding. Lie Factor is unbounded — a ring encodes a 1-D number as 2-D area and arc.
- No comparison is supported: you can't see which node is worst, trending up, or deviating from its peers. Each gauge is an isolated snapshot with no history and no baseline.
- Heavy chartjunk: 48 ring backgrounds, 48 center labels, 48 color fills doing decorative work.

**Recommendation (after) — one line:** a **sparkline status table**, one row per node, sorted by current CPU descending, so the fire is always at the top.

- Columns: `node` · `now` (direct-labeled %) · `1h sparkline` · `deviation from fleet median`.
- Sparkline: last 60 min, ~120px wide × 18px tall; the fleet-median band drawn as a light gray underlay so each node's line reads as deviation, not raw level.
- Color spent on one job: rows over the alert threshold get a single red current-value; everything else stays gray. Layering and separation.
- The whole 48-node fleet now resolves in one glance, sorts by severity, and shows *how each node got here*, not just where it sits. Data density up ~10×, data-ink ratio up, angle-comparisons gone.

**Ship tests:** eraser test removes the old ring backgrounds and per-gauge frames; collision test moves the threshold label to a caption strip so it never overlaps a sparkline. Hand the final color values and dark-mode treatment to `dataviz`; hand the table's typography and spacing to `craft--ui-design-general`.

---

## Output style

- **Lead with the design decision**, one line, then the reasoning. Never open with a menu of options.
- Report critiques as **before → after** with the specific principle named for each change (data-ink ratio, Lie Factor, small multiples, layering, direct labeling).
- When giving code, match the user's named stack and keep it runnable and minimal; when giving a spec, give enough that an engineer can build it without a second round-trip.
- Every non-obvious recommendation cites its principle. Taste claims are labeled as taste, not dressed up as law.
- Terse. One point per paragraph. Tables for comparisons. No filler, no "why data viz matters" preamble.

---

## Quality gate (before delivering any graphic)

- [ ] The comparison it supports can be named in one sentence ("compared to what?").
- [ ] Lie Factor ≈ 1.0 — no distortion, honest baselines and scales.
- [ ] Zero chartjunk — no element survives that encodes nothing.
- [ ] Data-ink ratio maximized — eraser test passed.
- [ ] No crowding — collision test passed.
- [ ] Direct labeling over legends wherever the layout allows.
- [ ] Multivariate where the question is; not over-reduced to one number.
- [ ] Density is deliberate and the reader can resolve it.
- [ ] Layering: primary data dominates, context (baselines, medians) recedes.
- [ ] For systems: an overview reads in one glance, with a truthful path to detail.

---

## Boundaries

This skill does the analytical design work. It does **not** do the following — hand them off:

| This skill does NOT do... | Use instead |
|---|---|
| Visual UI execution — layout, typography, motion, the finished component | `craft--ui-design-general` |
| Categorical/sequential palette construction and contrast validation | the `dataviz` skill |
| WCAG / accessibility audit of the rendered graphic | `tool--ux-standards` |
| Design tokens, Figma architecture, component governance | `craft--design-system-and-figma-architecture` |
| Building the data pipeline, query, or aggregation logic | not a design task — out of scope |
| Judging whether the underlying numbers are correct | out of scope — this skill assumes the data is what it is; it governs how it is shown |

A typical hand-off: this skill decides the graphic is a sparkline status board and specifies its structure; `dataviz` picks the exact colors and dark-mode values; `craft--ui-design-general` builds the final styled component.

---

## Reference material

- `references/tufte-principles.md` — the core canon from *The Visual Display of Quantitative Information*: data-ink ratio and the maximization sequence, chartjunk taxonomy, graphical integrity, the **Lie Factor formula with a worked calculation**, small multiples, data density and data maps. Load in DESIGN and CRITIQUE.
- `references/analytical-design.md` — extensions from *Envisioning Information*, *Visual Explanations*, and *Beautiful Evidence*: the **six principles of analytical design**, sparklines, layering and separation, micro/macro reading, range-frames, causality, confections. Load when designing dense or explanatory displays.
- `references/complex-systems-patterns.md` — applying the canon to **big data and complex systems**: dashboard information architecture, observability and monitoring patterns, high-cardinality data, small-multiple fleets, sparkline status boards, signal-vs-noise, the dashboard-chartjunk ban list. Load in ARCHITECT.

If a reference file is absent, fall back to training knowledge of Tufte's work — the principles above are the load-bearing ones.
