# Tufte Principles — *The Visual Display of Quantitative Information*

The core canon. These are the load-bearing rules for DESIGN and CRITIQUE.

---

## 1. Data-ink ratio

**Data-ink** is the non-erasable core of a graphic — the ink that represents the actual numbers. Everything else is non-data-ink.

```
data-ink ratio = data-ink / total ink used
```

Maximize it, within reason. The goal is not minimalism for its own sake; it is that every drop of ink should carry information.

**The maximization sequence (apply in order):**
1. Show the data. Establish the data marks first, alone.
2. **Erase non-data-ink.** Remove backgrounds, frames, heavy grids, decorative fills.
3. **Erase redundant data-ink.** A value shown by a bar's height does not also need a number on top *and* a gridline *and* an axis label at that height — pick the one that reads best.
4. Revise and edit. Restore only what the eraser test proves is load-bearing.

**The eraser test:** for each element, erase it. If no information is lost that isn't carried elsewhere, it stays erased.

---

## 2. Chartjunk

Ink that decorates instead of informs. The taxonomy to hunt and kill:

- **Heavy gridlines and frames** — replace a full grid with a few light ticks, or drop it entirely when values are labeled or on a range-frame.
- **Moiré / hatching / cross-hatching** — vibrating fill patterns that create optical noise. Use flat gray fills or none.
- **The duck** — a graphic where the decoration *is* the design, and the data is buried in it (named for a duck-shaped building). Any chart whose ornament dominates its data is a duck.
- **3D extrusion** — bars, pies, or surfaces given fake depth. Depth encodes nothing and distorts area/angle judgments. Never 3D a 2D dataset.
- **Drop shadows, bevels, gradients-as-decoration, background images.**
- **Redundant legends** when direct labeling would place the name at the data.

Chartjunk is not a matter of taste. Each item is ink spent on zero information — cut on sight.

---

## 3. Graphical integrity & the Lie Factor

A graphic has integrity when the visual effect matches the numerical effect.

```
Lie Factor = (size of effect shown in graphic) / (size of effect in the data)
```

Where each "size of effect" is a relative change:

```
size of effect = (second value − first value) / first value
```

**A graphic with integrity has a Lie Factor between ~0.95 and ~1.05.** Above ~1.05 it exaggerates; below ~0.95 it understates.

### Worked calculation

A bar chart shows fuel-economy standards rising from 18 to 27.5 mpg (a real Tufte example).

- **Effect in the data:** (27.5 − 18) / 18 = 0.528 → a **53%** increase.
- The graphic draws the "18" bar 0.6 inches long and the "27.5" bar 5.3 inches long.
- **Effect shown:** (5.3 − 0.6) / 0.6 = 7.83 → a **783%** increase.
- **Lie Factor** = 7.83 / 0.528 = **14.8**.

The graphic overstates the change nearly 15×. That is a lie, whatever its polish.

### Integrity rules
- The representation of numbers should be **directly proportional** to the numbers.
- **Bar charts start at zero.** Length is the encoding; a non-zero baseline breaks proportionality.
- Show data in **context** — a value alone can be made to look like anything; beside its comparison it tells the truth.
- **Number of dimensions in the graphic ≤ number of dimensions in the data.** Do not encode a 1-D number with 2-D area or 3-D volume — area/volume perception is unreliable and inflates the effect.
- Label thoroughly; deflate distortion with clear scales and annotation.
- Avoid dual y-axes engineered to manufacture a correlation.

---

## 4. Small multiples

A series of small charts, identical in structure and scale, indexed by a changing variable (time, entity, category). The eye learns the design once, then reads difference across the whole set at a glance.

**Rules that make them work:**
- **Same scale on every panel.** Different scales destroy comparison — the whole point.
- **Order by a meaningful variable** — magnitude, severity, geography, time — never alphabetically when a data order exists.
- **Shrink and repeat.** Small is the feature; density lets the eye sweep the set.
- Direct-label each panel; a shared caption states the scale once for all.

Small multiples are the single most powerful tool for "many of the same thing" — N services, N regions, N cohorts, N sensors.

---

## 5. Data density

```
data density = number of entries in the data matrix / area of the graphic
```

High density is a virtue when the reader can still resolve it — the human eye and a good display can carry enormous density (a map, a stock table, a train schedule). Most graphics are far too sparse: one number floating in a large tile wastes the reader's attention.

- **Maximize density, then use the eraser.** Shrink the graphic until it is as small as it can be while remaining legible.
- Under-reduction — a dashboard of single-value tiles — is the more common failure in software than over-crowding.
- Tables are high-density and often beat a chart for a small set of precise numbers a reader will look up.

---

## 6. Data maps & multivariate display

- Graphics can and should show **many variables at once** — Tufte's touchstones (Minard's map of Napoleon's march, the cancer-survival tables) carry 5-6 variables in one honest image.
- Do not strip a multivariate question down to a single series because one series is easier to draw. Over-reduction hides the structure the reader needs.
- Integrate words, numbers, and images in the same visual field rather than segregating a chart from its legend from its caption.
