# Surface Archetypes

Different site/app categories need different design rules — a dashboard, a newspaper, a portfolio, and an immersive panorama should never share the same typography, density, navigation model, or interaction pattern. Classify the surface FIRST (Phase 1 of the skill), then apply its row here — don't derive rules from a generic "modern web app" instinct.

Compressed from ~40 granular sub-types into governing categories. Sub-variants within a row (e.g. "blog" vs. "magazine" vs. "newspaper" within Editorial) differ mainly in density and expressiveness, not in governing principle — call that out in the brief rather than treating them as separate systems.

| Category | Primary user goal | Density | Navigation model | Key patterns | Sources |
|---|---|---|---|---|---|
| **Marketing & conversion** (corporate site, landing page, product marketing, lead-gen, event/launch) | Understand value, trust, convert | Low-medium | Minimal, CTA-anchored | Hero, proof/trust signals, repeated CTAs, feature storytelling, fast load for experimentation | General marketing UX practice |
| **Editorial & reading** (blog, magazine, newspaper, docs, knowledge base, long-form, newsletter) | Read, scan, continue, retrieve | Low (blog) → high (newspaper/docs) | Linear + section nav; docs need persistent left-nav/TOC | Narrow reading measure (~60 chars/line), strong heading hierarchy, low chrome, search-first for KB/docs | BBC GEL typography; NN/g F-pattern scanning |
| **Portfolio & visual presentation** (personal, photography, agency, architecture/property, museum/gallery) | Evaluate work, build impression, contact | Low, media-forward | Project/gallery-based, minimal chrome | Large accurate imagery, lightbox/fullscreen, case-study templates, gesture-safe navigation | General practice |
| **Commerce & transactions** (ecommerce, marketplace, booking/reservation, subscription) | Find, evaluate, buy/book | Medium | Search + category-first, persistent cart/account state | Findability (search, filters, category orientation), product media clarity, purchase-confidence signals (shipping/returns/trust), transactional state persistence | Baymard Institute research (homepage/category/product-page UX) |
| **Application products** (SaaS app, dashboard, CRM, admin, project/task mgmt, collaboration, data-entry, analytics/BI) | Complete repeat tasks, monitor, edit, decide | Medium-high | Persistent app shell nav; task-based, not content-based | Empty/loading/error/success states, KPI→chart→table drill-down, forms with draft/recovery, bulk actions, keyboard efficiency, permission-aware actions | IBM Carbon (grid/hierarchy for dense UI); Shopify Polaris (IA across surfaces); GOV.UK Service Manual (forms, workflow, task clarity) |
| **Utility & service surfaces** (search, map/location, calculator/configurator, file manager, auth, settings) | Complete one specific functional task | Medium | Task-embedded, not top-level | Immediate feedback, clear validation, empty/no-result states, reversible settings, keyboard-navigable | GOV.UK (forms, task completion) |
| **Storytelling & interactive** (interactive editorial, data story, timeline, educational, browser game) | Follow a guided narrative or complete a learning/play sequence | Variable, paced deliberately | Chapter/step-based, not persistent global nav | Scroll-driven reveal with reduced-motion fallback, annotation, progress/orientation cues, non-animated fallback for data stories | General practice; WCAG reduced-motion requirement |
| **Media & entertainment** (video platform, audio/podcast, gallery/media library, live/streaming) | Consume media, control playback | Low chrome around media | Persistent player, minimal competing UI | Captions/transcripts (WCAG), fullscreen/PiP, live-status handling, remote-control considerations when surfaced on TV | WCAG captions requirement |
| **Spatial & immersive** (360° panorama, virtual tour, WebGL/WebGPU, WebXR, spatial video) | Explore a space or experience, non-conventional interaction | N/A — governed by capability tiers, not density | Gaze/hand/controller/pointer, always with a non-immersive fallback | Explicit immersive entry/exit, capability detection, performance/GPU tiers, motion-comfort handling, conventional fallback is mandatory, not optional | Apple WebXR/visionOS developer docs |

## How to use this table

1. Classify the surface in Phase 1 of the skill's brief — ask if ambiguous, don't guess silently.
2. If a request spans two categories (e.g. a SaaS app with a marketing landing page), treat them as two surfaces with two different rows — never force one archetype's density/nav model onto the other.
3. Sub-variants (blog vs. newspaper, SaaS app vs. dashboard) differ in degree (density, pacing), not in kind — state the specific variant in the brief, but pull the governing row's principles.

## IA checklist for IA-heavy archetypes (Rosenfeld, Morville & Arango, *Information Architecture*)

Run this explicitly for **Editorial & reading** and **Application products** rows — these are the two archetypes where IA quality determines the product's success more than visual execution does. See `canonical-sources.md` for the full definitions.

1. **Organization system** — pick one primary grouping scheme (topic, task, audience, chronology) and state it; don't blend multiple schemes without a reason.
2. **Labeling system** — validate every nav label and heading against users' actual language, not internal terminology.
3. **Navigation system** — decide global/local/contextual structure before picking a visual pattern (tab bar vs. sidebar is the pattern; this is the underlying system that pattern expresses).
4. **Search system** — required, not optional, for any content-heavy or catalog-heavy surface (docs, KB, ecommerce catalog).
