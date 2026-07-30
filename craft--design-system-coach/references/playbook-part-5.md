# Design System Playbook — Part 5 of 5

*Modules covered: 6.1 – 6.4 (Governance)*

---

# Part 6 — Governance

## 6.1 Ownership and Roles

*One person owns the system. Multiple people contribute. Without a single owner, nobody owns it.*

Ownership is not a committee. The design system needs one person who is accountable for its health, even if many people contribute to it. As the team grows, the owner role grows with it, but it never disappears or splits.

### The roles

- **DS Owner** (always one person)
  - Accountable for the system's health
  - Approves promotions
  - Maintains the changelog
  - Schedules and runs Systems Day
- **Contributors**
  - Designers and engineers who build components into the system
  - Submit through the promotion gate
- **Consumers**
  - Anyone who uses the system in product work
  - Report bugs, request additions

### Roles by team size

| Team size | Owner | Contributors | Ambassadors |
|---|---|---|---|
| 1-2 designers | The lone designer | The same person | – |
| 3-4 designers | Senior designer (part-time) | All designers | – |
| 5-10 designers | Dedicated DS owner | All designers + senior eng | – |
| 10+ designers | Full-time DS owner + team | All designers + eng | One ambassador per product team |

### Ambassador model (at scale)

When the team passes 10 designers or 8 engineers:

- One ambassador per product team
- Ambassador represents the system in the team's rituals
- Ambassador surfaces requests, conflicts, divergence
- Ambassadors meet monthly with the DS owner

### Best practices

- The owner is named on every Library file Cover page
- Ownership transitions are logged in the changelog
- The owner has dedicated time, not "stolen" time

### Common mistakes

- "Everyone owns the system" (nobody does)
- Rotating ownership monthly (kills continuity)
- Treating the owner role as part-time forever

### Rule of thumb

If you cannot name the DS owner in one breath, the system has no owner.

**Sources:** Mall S4, Couldwell S6.

---

## 6.2 Changelog and Decision Log

*Two living artifacts. One captures what changed. The other captures why.*

The changelog and decision log are not optional documentation. They are part of the system itself. In a regulated company, they are also part of the audit trail. Both live inside Figma so they are always with the artifact they describe.

### Changelog (per file)

Lives on the Changelog page of every Library file.

| Date | What changed | Who | Why | Reviewer |
|---|---|---|---|---|
| 2026-04-22 | Added critical alert variant | A. Solis | Clinical protocol update | Dr. Bianchi |
| 2026-04-15 | Renamed `Snackbar` → `Toast` | A. Solis | Aligned with code naming | Lead Eng |

### Decision log (per component)

Lives on the component page itself.

| Field | Example |
|---|---|
| Decided | Use red 600 for critical alerts |
| Why | Highest contrast that passes WCAG AA |
| Rejected | Red 700 (too dark in dark mode), orange (warning conflict) |
| Date | 2026-04-22 |
| Reviewer | Dr. Bianchi (clinical), A. Solis (design) |
| Linked docs | MDR usability file §3.4 |

### Difference between the two

- **Changelog** answers: what changed, when, by whom
- **Decision log** answers: why this decision and not another

### When entries are required

- Always: any change to a Library file
- Always: any new component promotion
- In regulated work: any change to a clinical or safety-related component, with reviewer sign-off

### Best practices

- Entries are short but specific. "Updated alert" is too vague. "Added critical variant for vitals out of range" is right.
- Newest at the top
- One row per change, never batched

### Common mistakes

- Treating logs as "we will fill them later"
- Entries with no reviewer
- Vague entries that do not say what actually changed

### Rule of thumb

If a regulator asked "why does the alert use this red?" the decision log should answer in 30 seconds. If it cannot, the log is not doing its job.

**Sources:** Couldwell S6, Skill body.

---

## 6.3 Retrofit Sequence

*The team has a messy current state. Fix it without stopping product work. Two parallel tracks.*

A retrofit is what happens when a design system already exists in some form but is broken, inconsistent, or partial. The goal is to migrate to the clean structure without pausing feature delivery. Two parallel tracks run continuously: new work on the new structure, retrofit work in scheduled blocks.

### The two tracks

- **Track A — New work**
  - All new features use the clean structure from day one
  - New components go into Playground and graduate normally
  - Legacy library is read-only for new work
- **Track B — Retrofit**
  - During Systems Day, migrate one or two existing components from legacy into the clean library
  - Mark legacy versions as deprecated, do not delete (compliance retention)
  - Update product files to consume the new versions component by component

### Priority order for retrofit

1. **Regulated components first** — vitals, alerts, clinical forms, consent flows
2. **High-traffic components second** — Button, Input, Modal, Toast
3. **Domain-specific components third** — anything used in only one product
4. **Polish and edge cases last** — tooltips, badges, micro-utilities

### Sequence example ([Client], weeks 1-8)

- Week 1: Stand up Foundations file. Extract every token from the legacy library.
- Week 2: Migrate one regulated component end-to-end (e.g. `Vitals Alert`). Engineering confirms.
- Week 3: Promote it. Update RPM Pro to consume the new version. Deprecate the old.
- Weeks 4-8: Repeat for next priority components, one per Systems Day.

### Best practices

- Never pause Track A to finish Track B
- Migrate component by component, not all at once
- Keep legacy in read-only state, not deleted
- Each migrated component has a fresh decision log entry

### Common mistakes

- Trying to fix everything in one sprint
- Deleting the legacy library before migration is complete
- Mixing legacy and clean components inside the same file

### Rule of thumb

A good retrofit is invisible to product. The team feels no slowdown. The system gets cleaner one component per Systems Day.

**Sources:** Mall S4 (pilot approach), Couldwell S6 (audit + iterative migration).

---

## 6.4 When to Fork, When to Extend

*Most of the time, extend. Fork only when extending breaks the system.*

As the system grows, new requirements arrive: a new product, a new platform, a new brand, a new partner. The decision is whether to extend the existing system (a new mode, a new component, a new file) or to fork it (a separate library that diverges). Forks are expensive forever. Extensions cost almost nothing.

### Extend when

- The new requirement is a brand variation → new Tier 2 mode
- The new requirement is a platform variation → new Components file, same tokens
- The new requirement is a domain variation → new Domain file, same Foundations
- The new requirement is a product variation → new product file, consume the same library

### Fork when

- The new requirement has fundamentally different governance (e.g. a regulated product taken on by a non-regulated parent company)
- The new requirement uses a different component library that cannot share Tier 1 or Tier 2
- A partner brand has demanded entirely different component behaviour, repeatedly, and the override surface exceeds 50% of the library

### The "third partner" rule

For multi-brand work: do not fork the system for the first or second partner. Add Tier 2 modes. Wait for the third partner to ask for the same kind of divergence. If three partners ask for the same fork, it is a real pattern. Two is coincidence.

### Canon vs Expanded Universe

- **Canon** — the core system. Shared by all products and brands.
- **Expanded Universe** — product-specific extensions. Lives outside Canon. May reference Canon but does not change it.

When in doubt, put new work in Expanded Universe first. Promote to Canon only when multiple products consume it.

### Best practices

- Default to extension. Fork is the last resort.
- Document every fork decision with rejected alternatives.
- Forks are owned. They do not become orphans.

### Common mistakes

- Forking on the first partner request
- Forking instead of investing in better tokens
- Treating Expanded Universe items as second-class

### Rule of thumb

A fork doubles the maintenance cost forever. An extension costs almost nothing. Choose accordingly.

**Sources:** Mall S4 (Canon vs Expanded Universe), Frost S3 (multi-all-the-things), our synthesis on the third-partner rule.

---

# Sources Cited

| Source | Author | Work |
|---|---|---|
| S1 | Brad Frost | *Atomic Design* (2016) |
| S2 | Brad Frost | *Design System Ecosystem* (2023) |
| S3 | Brad Frost | *Multi-All-The-Things* (2025) |
| S4 | Dan Mall | *Design That Scales* (2023) |
| S5 | Alla Kholmatova | *Design Systems* (2017) |
| S6 | Andrew Couldwell | *Laying the Foundations* (2019) |

---

End of playbook.
