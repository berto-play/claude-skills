# Source 5: Design Systems — Alla Kholmatova (2017)

**Confidence: HIGH — Full book extracted and verified.**

---

## Table of Contents

- [Definition](#definition)
- [Two Types of Patterns](#two-types-of-patterns)
- [Three Parameters to Characterise Any System](#three-parameters)
- [Design Principles](#design-principles)
- [Naming Patterns](#naming-patterns)
- [Shared Language Practices](#shared-language-practices)
- [Purpose-Directed Interface Inventory](#purpose-directed-interface-inventory)
- [Pattern Documentation Minimum](#pattern-documentation-minimum)
- [The Moral Reminder](#the-moral-reminder)

---

## Definition

A design system is a set of connected patterns and shared practices, coherently
organised to achieve the purpose of digital products. Not a component library.
Not a style guide. A set of practices that serves a purpose.

---

## Two Types of Patterns

Use this taxonomy always when classifying patterns.

**Functional patterns:** The tangible building blocks. Buttons, forms, tables,
navigation. Their purpose is to enable or encourage certain user behaviours.
They are shaped by the domain the product belongs to. A trading platform's
functional patterns (data grids, charts, dense controls) are fundamentally
different from an education platform's (articles, discussion threads, progress
indicators). Patterns are the physical embodiment of the behaviours you are
trying to encourage.

**Perceptual patterns:** How a product is felt. Typography, colour, spacing,
iconography style, animation, tone of voice. Even a purely functional tool has
an aesthetic. Perceptual patterns are not decoration — they are differentiators.
Spotify and Apple Music have similar functional patterns. Perceptual patterns
are why they feel nothing alike.

---

## Three Parameters

Use these to characterise every system. There is no correct answer. The right
system is the one where you can manage the downsides. Conway's Law applies:
your system will mirror your communication structure.

| Parameter | Axis | When to use each end |
|-----------|------|---------------------|
| Rules | Strict ↔ Loose | Strict for large teams needing predictability. Loose for small teams with deep shared culture. |
| Parts | Modular ↔ Integrated | Modular for products that scale, evolve, and have parallel teams. Integrated for one-offs, campaigns, art-directed experiences. |
| Organisation | Centralised ↔ Distributed | Centralised for ownership and curation. Distributed for autonomy and resilience. Hybrid for large orgs. |

---

## Design Principles

Four qualities that make design principles work:

1. **Authentic and genuine.** "Simple. Useful. Enjoyable." applies to every
   product on earth. Good principles are specific to your product.

2. **Practical and actionable.** A principle should help answer a real design
   question. "No needless parts" is more useful than "Make it simple."

3. **Have a point of view.** Principles should resolve tension, not describe
   ideals. Salesforce Lightning: Clarity, Efficiency, Consistency, Beauty —
   applied in that priority order. Clarity always wins.

4. **Relatable and memorable.** Three to five principles maximum. If no one
   can recall them, they are not working.

---

## Naming Patterns

The most underrated skill in design systems.

- If you struggle to name a pattern, the pattern's purpose is unclear. That
  is a red flag, not a naming problem.
- Good names are based on metaphors (FutureLearn's "Billboard," "Brackets,"
  "Spotlight").
- Good names have personality ("Boss," "Minions," "Whisperbox," "Boombox").
- Good names communicate purpose and guide usage. "Boss" tells you there can
  only be one per page.
- Naming is cross-disciplinary. Designers, engineers, and content people see
  the same module differently. All three perspectives produce better names
  than one.
- Presentational names ("Circle with a dot," "Prominent tile") fail at scale.
  They give no guidance on where to use a pattern.

---

## Shared Language Practices

These are operational, not aspirational.

- **Pattern wall:** Print screens, put them on the wall, label the patterns.
  Makes the system visible.
- **Dedicated naming channel:** Post a screenshot, describe the purpose, invite
  names. Celebrate good ones.
- **Regular catch-ups:** Fortnightly, 30 minutes, structured agenda. Keep
  everyone on the same page.
- **Glossary:** Write it down. The act of writing forces clarity.
- **Induction:** New team members learn the pattern language on day one.
- **Use the names in conversation.** Until you call it "Whisperbox" out loud,
  it does not exist in the system.

---

## Purpose-Directed Interface Inventory

Standard interface inventories group by appearance (all the buttons together).
Purpose-directed inventories group by behaviour (all the patterns that support
"discovering content" together). Two buttons that look identical might serve
different behaviours. Two modules that look different might serve the same one.
This distinction matters for how you define and name patterns.

Run an inventory before defining patterns. Not after. Not alongside. Before.

---

## Pattern Documentation Minimum

Every pattern needs at least:

1. **Name** — prominent, stands alone
2. **Purpose** — one or two sentences (harder than it sounds)
3. **Example** — live instance preferred; GIF for interactions
4. **Variants** — presented as a suite, with differences explained

Add: versioning, related patterns, team members, guiding principles. But start
with the four above.

---

## The Moral Reminder

> "Will these patterns make human life better as a result of their injection
> into the system?"

