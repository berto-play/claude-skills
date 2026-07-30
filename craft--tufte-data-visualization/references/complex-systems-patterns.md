# Complex Systems & Big Data — Applying the Canon

Tufte's principles applied to the subject this skill defaults to: systems with a lot of data and a lot of moving parts — observability stacks, distributed services, fleets, high-cardinality feeds, multi-page operational reports. Load in ARCHITECT.

The through-line: **the hard problem in a complex system is not drawing a chart, it's information architecture** — deciding what the reader sees, in what order, at what density, so that truth about the whole system is legible at a glance and detail is one step away.

---

## Information architecture: overview → detail, dense the whole way

Combine Shneiderman's mantra — *overview first, zoom and filter, details on demand* — with Tufte's density. The mistake is to read "overview first" as "start sparse." Every level should be dense and truthful:

1. **Level 0 — the glance.** In one screen, the reader answers: is the system healthy? Where is the worst of it? This is a small-multiples grid or a sorted sparkline board, not a row of single-number tiles.
2. **Level 1 — the entity.** Drill to one node/service/region: its key series over time, against its baseline, with related signals layered in.
3. **Level 2 — the moment.** The specific event: logs, traces, the exact values at the spike.

Design the *path* between levels as deliberately as the panels. A reader should never wonder where to look next.

---

## Pattern library

### Small-multiple fleet
**Use when:** you have N of the same thing (services, nodes, regions, cohorts, sensors) and need to compare them.
- One panel per entity, **identical structure, one shared scale**.
- **Order by severity or magnitude**, not name — the worst panels cluster where the eye lands first.
- Layer each entity's line over a light fleet-median or SLO band so each panel reads as *deviation*.
- Scales beat pagination: 60 small panels on one screen out-inform 60 clicks.

### Sparkline status board
**Use when:** you need current state *and* recent history for many entities at once.
- Table: `entity` · `now` (direct-labeled) · `sparkline (recent window)` · `deviation / trend`.
- Sort by the column on fire. The board self-triages.
- One accent color for breaches; everything else gray. Layering does the work.
- This is usually the right answer when someone shows you a wall of gauges.

### Horizon chart
**Use when:** you must show many time-series in very little vertical space (dense server metrics).
- Bands of increasing value are folded and color-layered so a tall chart's information fits in a short strip. High density, preserves shape. Use sparingly — it has a learning cost; label it.

### Time-series at scale
- **Aggregate honestly.** When downsampling a million points to a thousand pixels, show min/max envelopes or use a shape-preserving downsample (e.g. LTTB) — never a naive average that erases spikes. A hidden spike is a lie of omission.
- **Show gaps.** Missing data is a break in the line, not a straight interpolation across it.
- Band the normal range as a light underlay; the live line is the dark foreground.

### High-cardinality data
- You cannot show 10,000 series. Decide the comparison, then **rank and cut**: top-N by the metric that matters, with the long tail collapsed into one "other" aggregate that is clearly labeled as such.
- Or shift to a **distribution** view (histogram, ECDF, heatmap of value × time) when individual series stop mattering and the shape of the population is the question.

### Part-to-whole across a system
- A stacked area over time hides individual trends inside the stack. If per-part trends matter, use **small multiples**, not a stack.
- Pies and donuts are almost always wrong here — angle comparison is inaccurate and they don't scale past a few slices.

---

## Signal vs. noise

- **The graphic's job is to raise signal, not to alarm.** A dashboard where everything blinks red teaches the reader to ignore red.
- Encode *deviation from expected*, not raw level, wherever a baseline exists — the reader cares that CPU is 40 points above this node's own normal, not that it is at 80.
- Reserve the strongest visual weight (color, motion, size) for the one state that demands action. Spend it once.

---

## The dashboard-chartjunk ban list

These are the ducks of operational dashboards. Cut on sight; replace with the honest form.

| Banned | Why it fails | Replace with |
|---|---|---|
| Donut / radial gauge for a single % | 2-D area + angle to encode a 1-D number; unbounded Lie Factor; no history | A number in context + a sparkline |
| Single-value "KPI" tile, no comparison | Answers no "compared to what?"; wastes the whole tile | The number beside its target, trend, or prior period |
| Speedometer / dial | Ornament dominates data; imprecise | Bar against a threshold, or a labeled value |
| 3-D pie, exploded pie | Distorts every proportion; angle is the worst encoding | Bar or table |
| Rainbow heatmap | Non-perceptual color order invents structure that isn't there | Sequential (single-hue) or diverging-about-a-midpoint scale |
| Full gridlines behind every panel | Chartjunk × number of panels | Light range-frame or a single reference line |
| A legend beside a small-multiple grid | Redundant; forces eye round-trips | Direct-label each panel |
| Auto-refresh animation with no state memory | Motion encodes nothing; hides the trend it replaces | A static sparkline showing the actual history |

Hand the **color** decisions (which sequential scale, dark-mode values, contrast) to the `dataviz` skill and the **layout/typography** to `craft--ui-design-general`. This file owns the analytical choice: what form is honest and legible for the data.
