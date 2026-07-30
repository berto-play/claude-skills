# Design System Playbook — Part 3 of 4

*Modules covered: 4.1 – 4.4 (Token Architecture and Foundations)*

---

# Part 4 — Foundations

## 4.1 Token Architecture

*Three tiers. The component library is a rental. The tokens are what you own.*

Tokens are the smallest decisions in the system: a colour, a spacing value, a font size. They are organised in three tiers, each with a different job. The tier system is what makes the design system survive a change of component library, a new brand, or a regulatory update without a rebuild.

### The three tiers

```
TIER 1 (Primitives)    Raw values. No meaning. Ingredients only.
        ↓
TIER 2 (Semantics)     What the values mean in context. The design language.
        ↓
TIER 3 (Component)     What specific components consume. Library-adjacent.
```

### Reading the chain

Each token resolves to the next tier:

```
TIER 1   color/primitive/red/600              → #d92d20
TIER 2   color/semantic/feedback/error        → color/primitive/red/600
TIER 3   component/alert/error/background     → color/semantic/feedback/error
```

### What each tier is for

- **Tier 1** — the raw colour scale, spacing scale, typography scale. Changes only when the brand fundamentally changes.
- **Tier 2** — what a value means: primary action, error feedback, surface background, clinical critical. Where brand modes and white-label modes live.
- **Tier 3** — what each component consumes. Aliases Tier 2. The only tier that changes when the component library is replaced.

### The migration rule

When the library changes, only Tier 3 remaps. Tier 1 and Tier 2 stay untouched. This is what makes a library migration weeks instead of months.

### Domain tokens (regulated products)

In regulated products, add a domain semantic category in Tier 2, separate from feedback tokens.

```
color/semantic/feedback/error       → form validation
color/semantic/clinical/critical    → patient safety
```

Same primitive value today. Different meanings. Easy to differentiate tomorrow.

### Best practices

- Components reference Tier 3 only. Never Tier 2 or Tier 1.
- Tier 2 is the only tier that has brand modes.
- Tier names read left-to-right as "narrows to."
- Document the mapping chain inside the Foundations file.

### Common mistakes

- Skipping Tier 3 and applying Tier 2 directly to components (locks you in)
- Using Tier 1 values in components (raw hex in code or Figma)
- Mixing brand overrides into Tier 1 instead of Tier 2

### Rule of thumb

If a library swap requires touching anything other than Tier 3, the architecture is wrong.

**Sources:** Frost S2, Skill body.

---

## 4.2 3rd-Party Libraries

*Own the tokens. Rent the library.*

A third-party component library (Material UI, Ant Design, Chakra, Tamagui, React Native Paper) is a rental. The team owns the tokens, the domain components, and the governance. The library provides generic primitives. When the library changes or gets replaced, the system must survive without a rebuild.

### What stays yours

- Tier 1 primitives
- Tier 2 semantics, including domain tokens
- Domain (Clinical) library
- Playground
- Decision log

### What you do not rebuild

- Generic components from the library: Button, Input, Modal, Toast
- Use the library's official Figma kit, not a hand-rebuild

### File impact

| Without 3rd-party library | With 3rd-party library |
|---|---|
| Components Web, Components Mobile | Library's Figma kit, plus Overrides files |
| 5 library files | 3-4 library files |

### Questions to ask before adopting one

1. **Does the library cover at least 70% of our component needs?** If not, the override surface will outweigh the rental savings.
2. **Is the Figma kit officially maintained or community-maintained?** Community kits drift fast.
3. **Does the library expose theme tokens cleanly?** If we cannot map Tier 3 to its theme, the migration insurance is gone.
4. **What is the upgrade cost?** Read the changelog of the last two major versions.
5. **Does it work for both web and React Native?** If not, we need two libraries.

### Best practices

- Never fork the library's source.
- Override via theme tokens, never inline styles.
- Compose on top for domain-specific components, never inside.
- Pin the library version in the Foundations file.

### Common mistakes

- Rebuilding library components in Figma (waste of weeks)
- Over-customising the library (if you style 80% of properties, wrong library)
- Skipping Tier 3 tokens
- Not tracking library version

### Rule of thumb

Pick the library whose defaults are closest to where you want the product to be. The right library saves you from styling. The wrong library makes you fight it.

**Sources:** Skill principle 7, Couldwell S6.

---

## 4.3 Tracking a 3rd-Party Library

*If a 3rd-party library is in the system, the system must say so. Out loud. In writing.*

When the team adopts a 3rd-party library, future-you needs to know which one, which version, who decided, and why. Without this, library migrations become archaeology.

### Where it is documented

**Foundations file Cover page** — mandatory fields when a library is adopted:

- Library name
- Version pinned (matching `package.json`)
- Link to library documentation
- Link to the official Figma kit
- Adoption decision date
- Adoption reviewer (design + engineering)

### Changelog addition

Add a `Library version` column to the file changelog. Every upgrade is a logged entry.

| Date | What changed | Who | Why | Reviewer | Library version |
|---|---|---|---|---|---|
| 2026-04-22 | Upgraded MUI 5 → 6 | A. Solis | Security patch + new components | Lead Eng | 6.0.2 |

### Decision log entry (one-time, on adoption)

| Field | Required content |
|---|---|
| Decided | Adopt [library name + version] |
| Why | Coverage, theme support, ecosystem fit |
| Rejected | Other libraries considered, with reason |
| Migration cost estimate | Time to swap if needed in the future |
| Reviewer | Designer + Engineering Lead |
| Date | ISO date |

### Engineering side

- Library version pinned in `package.json`
- Design and code versions must match
- A library version mismatch between Figma and code is treated as a bug

### Best practices

- Document the library on adoption, not later
- Update the Cover page on every version bump
- Keep the rejected-alternatives list for future reference

### Common mistakes

- Adopting a library without a Cover entry
- Letting Figma and code drift on version
- Forgetting to log the rejected alternatives

### Rule of thumb

If the team cannot answer "which library, which version, who decided" in one sentence, the documentation is missing.

**Sources:** Our synthesis. Skill covers Tier 3 remapping; the documentation pattern itself is operational guidance.

---

## 4.4 White-Labeling

*A new brand is a new mode in Tier 2. Never a new file.*

White-labeling is when the same product is shipped under another brand. Done right, the design system supports it without forking files or duplicating components. Brand differences live in token modes. Components stay shared.

### What varies per brand

- Tier 1 primitives if the partner brand has fundamentally different palette or typography
- Tier 2 semantics (always, this is the override layer)
- Brand assets: logo, illustration style

### What stays shared

- Component structure and behaviour
- Spacing, density, layout
- Domain tokens (clinical meaning is universal)
- Accessibility requirements
- Naming conventions

### How brand modes work

In Figma, Tier 2 lives inside a variable collection with one mode per brand:

```
Collection: Color Semantic (Tier 2)
├── Mode: [Client] (default)
│   ├── semantic/primary        → primitive/blue/600
│   └── semantic/feedback/error → primitive/red/600
├── Mode: White-label A
│   ├── semantic/primary        → primitive/green/500
│   └── semantic/feedback/error → primitive/red/600
```

Switching the mode on a frame re-themes the entire design.

### Questions to ask when extending

1. **Is this brand asking for visual changes, or behavioural changes?** Branding is appearance only. If they want different behaviour, that is a product decision, not a brand one.
2. **Can we satisfy them with a Tier 2 mode?** If yes, that is the answer.
3. **Are they asking for entirely different components?** If yes, those go into a Brand Overrides file, never into the core library.

### Best practices

- New brand = new Tier 2 mode
- Components reference Tier 2, never raw values
- Do not pre-build modes for partners we do not have yet
- Brand overrides live in their own file, not inside Components

### Common mistakes

- Creating a separate Figma file per white-label (drifts within weeks)
- Hard-coding brand colours in components
- Letting partners drive component behaviour
- Filling placeholder modes with guessed values

### Rule of thumb

Branding is appearance. Product is behaviour. Do not mix them.

**Sources:** Frost S3, Mall S4 (Canon vs Expanded Universe).

---

## 4.5 Token Pipeline

*Tokens are authored in Figma. The product ships code. There is always a pipeline between them.*

### The pipeline

```
Figma Variables / Tokens Studio → JSON export → Style Dictionary → CSS / JS / React Native
```

### Authoring layer

Two options — choose one per system, not both:

- **Figma Variables (native):** Built into Figma since 2023. Supports Tier 1 and Tier 2 (colour, number, string, boolean). No W3C export yet; requires a plugin or CI script to extract.
- **Tokens Studio plugin:** Extends Figma Variables with W3C-compliant JSON, multi-value tokens (shadows, typography composite tokens), math expressions, and direct sync to a GitHub repo.

### Transformation layer

Style Dictionary (open source, Amazon) consumes the token JSON and outputs to any target:

```
token JSON → Style Dictionary → CSS variables, JS/TS module, Swift, Kotlin, React Native StyleSheet
```

### CI integration

Token changes trigger the pipeline automatically. A Tokens Studio commit or a GitHub Action on the token repository exports and transforms without manual steps. Designers do not hand JSON to engineers.

### Common mistakes

- Exporting tokens manually per release (brittle; diverges fast)
- Skipping Style Dictionary (each platform hand-codes its own values)
- Storing raw hex in code rather than consuming CSS variables

**Sources:** W3C Design Token specification, Tokens Studio docs, Style Dictionary docs.

---

## 4.6 Code Connect

*Figma's Inspect panel shows CSS approximations by default. Code Connect replaces them with real production code.*

Code Connect is Figma's native mechanism for linking a Figma component to its code implementation. Once connected, selecting a component in Inspect shows the actual import statement and usage snippet — not a CSS approximation.

### How it works

1. Engineering annotates each code component with a `figma.connect()` call (TypeScript or React).
2. The annotation is pushed to Figma via the Code Connect CLI: `figma connect publish`.
3. In Inspect, selecting a connected component renders the real snippet.

### What it enables

- **Accurate handoff:** Designers share frames; developers see production-ready code, not guessed styles.
- **AI-assisted generation:** Figma AI, Claude, and Copilot use Code Connect mappings to produce correct component usage — reduces hallucinated prop names.
- **Canonical naming bridge:** Creates a permanent, machine-readable link between design and code identifiers.

### When to skip it

If the system uses an unmodified 3rd-party library kit, Code Connect is the library team's job. Apply it only to domain-specific (Tier 3 / Clinical) components the team owns.

### Setup cost

One annotation file per component. Lightweight for new systems; worth running as a focused sprint for legacy systems with 30+ components.

**Sources:** Figma Code Connect documentation (2024).

---

## 4.7 Code-Kit vs. Hybrid — Decision Framework

*Where does truth live — in Figma, in code, or in both? This choice sets the collaboration model for the life of the system.*

### The three models

**Design-led (Figma as truth)**
Figma is the authoritative source. Code mirrors what design publishes. Handoff is the primary workflow. Works when designers outnumber engineers 2:1 or more, or when the product is highly design-driven.

**Code-led (Storybook as truth)**
Code is authoritative. Figma (if it exists) is documentation only. Engineers publish components; designers adapt. Works for engineering-heavy teams shipping at high velocity.

**Hybrid (tokens as the sync layer)**
Design and code maintain separate artefacts, kept in sync via the token pipeline and Code Connect. Neither side owns the other. Requires both Modules 4.5 and 4.6 to be operating. Works when the team is balanced and the system is mature enough to maintain both.

### Decision matrix

| Factor | Design-led | Code-led | Hybrid |
|---|---|---|---|
| Designer : Engineer ratio | 2:1 or higher | 1:3 or more | ~1:1 |
| Release cadence | Slower (design gates) | Fast (code drives) | Balanced |
| Component count | <50 | Any | 50+ |
| Regulated product | Risky (design can drift) | Preferred | OK if token pipeline is tight |
| Token pipeline needed | Optional | Not applicable | Required |
| Code Connect needed | Optional | N/A | Required |

### The hidden cost of each model

- **Design-led:** Drift begins when a developer ships a fix without updating Figma. Audit debt accumulates silently.
- **Code-led:** Designers lose visibility. New feature design happens in Storybook previews or screenshots. UX iteration slows.
- **Hybrid:** Requires pipeline investment upfront. If the pipeline breaks, both sides drift independently — harder to detect than design-led drift.

### Rule of thumb

Starting a new system: go hybrid. The upfront token pipeline cost pays back in the first major redesign. Taking over a mature code-led system: invest in tokens first, Code Connect second.

**Sources:** Couldwell S5, Frost S4, engineering collaboration module synthesis.

---

Continued in playbook-part-4.md. (Parts 5 and 6 — Workflow and Governance: modules 5.1–6.4)
