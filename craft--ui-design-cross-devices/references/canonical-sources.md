# Canonical Sources — Non-Negotiable Rules

Research-backed rules, not house opinion. Each is tagged to its source. Use this file to ground a decision instead of inventing psychology from training-data instinct.

## Interaction laws (still cited today)

| Law | What it says | Source |
|---|---|---|
| **Fitts's Law** | Time to acquire a target is a function of distance and size — bigger, closer targets are faster to hit | Cited across HCI research; formalized in mobile/large-screen guidance (Material 3, Apple HIG) |
| **Hick–Hyman Law** | Choice reaction time increases with number/complexity of options | Basis for "limit options, simplify menus" guidance industry-wide |
| **Miller's chunking** | Working memory holds a small number of chunks reliably (7±2 debated, but the chunking principle holds) | Basis for grouping dense content into labeled chunks of 3-5 items, not flat lists |
| **Jakob's Law** | Users spend most of their time on other sites/apps and expect yours to work the same way | Nielsen Norman Group — favor established conventions over novel patterns |
| **Progressive disclosure** | Reveal detail only when the user asks for it | Canonical pattern from constrained-screen era (feature phones), formalized in Material 3 adaptive design |
| **Doherty Threshold** | Productivity rises disproportionately once system response time drops under 400ms — the older "2 second" tolerance standard is wrong | Doherty & Witteman, 1982 (IBM), cited in Yablonski's *Laws of UX*. When true processing exceeds 400ms, provide feedback immediately (skeleton screens, progress indicators) to create the *perception* of a faster system — this is the actual justification for a loading state, not just "because the checklist says so" |
| **Tesler's Law (Law of Conservation of Complexity)** | Every process has an irreducible amount of complexity — the only decision is who absorbs it, the system or the user | Larry Tesler, cited in Yablonski's *Laws of UX*. Directly governs the application-product archetype: a "simplified" checkout or settings flow that hides a required decision doesn't remove the complexity, it dumps it on the user later, usually as confusion or a support ticket |
| **Von Restorff Effect** | An item that is visually or conceptually distinct from its surroundings is disproportionately memorable | Hedwig von Restorff, 1933, cited in Yablonski's *Laws of UX*. Justifies exactly one emphasized element per view (a highlighted pricing tier, one accent-colored primary action) — using it on multiple elements in the same view cancels the effect out |
| **Gestalt: Proximity** | Objects placed close together are perceived as a group | Yablonski's *Laws of UX*, classic Gestalt psychology — use spacing, not borders, as the first tool for showing relatedness |
| **Gestalt: Similarity** | Objects that share visual traits (color, shape, size) are perceived as related | Yablonski's *Laws of UX* — keep visual treatment consistent within a semantic group (e.g. all destructive actions styled the same way) |
| **Gestalt: Common Region** | Elements sharing an enclosed boundary (a card, a bordered panel) are perceived as one group, even overriding proximity | Yablonski's *Laws of UX* — a bounded card groups its contents more strongly than spacing alone; use deliberately, not on every element by default |
| **Gestalt: Prägnanz** | People perceive ambiguous forms in the simplest way possible | Yablonski's *Laws of UX* — favor simple, resolvable shapes and layouts over ones that require the eye to reconcile competing interpretations |

## Affordances and signifiers (Don Norman, *The Design of Everyday Things*)

Distinct from Fitts's/Hick's — those are about speed and choice; this is about whether the user can perceive an interaction is even possible at all.

- **Affordance**: a property of an object that permits an action (a raised element affords pressing; a scrollable list affords swiping). The affordance either exists or doesn't — a button that looks unpressable but is clickable has a broken affordance regardless of how "clean" the visual design is.
- **Signifier**: the perceivable signal that communicates an affordance exists (a drop shadow suggesting elevation and pressability, an underline suggesting a link). Signifiers are what a designer actually controls — the affordance may exist in code, but without a signifier the user cannot discover it.
- **Mapping**: the relationship between controls and their effects should match the user's mental model of the world (a volume slider that moves up for louder, a light switch flipped up for on).
- **Cross-device stakes**: a signifier that works on a mouse-hover surface (subtle color shift on hover) has no equivalent on touch or D-pad — if the *only* signifier for an affordance is a hover state, that affordance is invisible on touch and TV. Every interactive element needs a signifier that survives its input model, not just its default (pointer) one.

## Designing component states (Dan Saffer, *Microinteractions*)

The Quality Gate requires loading/error/empty/success states to exist — this section is how to actually design one, not just check a box.

Every microinteraction (a single task: toggling a setting, liking a post, submitting a form) has four parts:

1. **Trigger** — what starts it. Can be manual (a tap) or system-initiated (an automatic sync). A trigger needs a clear signifier (see above) if manual, and should be unsurprising if system-initiated.
2. **Rules** — what happens, in what order, and what's allowed. Define these explicitly before designing the visuals: what can the user do while this is in progress? Can they cancel? What's the one and only valid path?
3. **Feedback** — how the rules are made perceivable as they play out (a spinner, a color change, a sound, a haptic). Feedback should communicate system status continuously, not just at the end — this is what makes a Doherty-Threshold-driven loading state actually work.
4. **Loops and modes** — what happens on repeat use (does it get faster/smarter?), and whether a mode changes the rules temporarily (e.g. airplane mode changing what a "send" trigger does).

**Applied to the Quality Gate's required states:**
- **Loading**: trigger = action started; feedback must appear within the Doherty Threshold (~400ms) or perceived performance craters even if real performance is fine.
- **Error**: rules must define what's recoverable vs. fatal; feedback must say what happened and what to do next, not just that something failed.
- **Empty**: the state before any trigger has fired — still needs a signifier telling the user what action would fill it (don't just show blank space).
- **Success**: often the most neglected — a confirmed action deserves feedback proportional to its importance (a deleted item can fade silently; a completed purchase needs unambiguous confirmation).

## Navigation — sourced, not opinion

- **Bottom tab bar**: correct for 3-5 top-level destinations of similar importance/peer status. Apple HIG describes tab bars as presenting peer categories; Material Design frames bottom navigation the same way — for direct access to top-level destinations, not actions or settings. (Apple HIG, Material Design)
- **Hamburger/drawer**: correct only for secondary, infrequent, or too-numerous-to-fit destinations. NN/g's research found hiding main navigation in a hamburger measurably reduced discoverability and increased task difficulty. The evidence-based rule is not "never use a hamburger" — it's "never hide core/high-frequency destinations inside one." (Nielsen Norman Group, "Hamburger Menus and Hidden Navigation Hurt UX Metrics")
- **Stack/hierarchical navigation**: correct for parent-child drill-down flows (settings, catalogs, detail screens). Apple HIG recommends one clear path to each screen. (Apple HIG)
- **What shifted in the last 5 years**: hamburger menus didn't become "forbidden" — their justified use narrowed to secondary navigation only. Bottom navigation became more normalized as mobile-first design matured, because it aligns with thumb reach and high-frequency destinations. (NN/g, industry trend)

## Failure patterns (named anti-patterns, with evidence)

- **Mystery meat navigation**: users can't tell what's tappable without poking around — forces guesswork, breaks recognition-over-recall.
- **Hidden-nav discoverability collapse**: NN/g directly measured lower discoverability and higher task difficulty when primary navigation is hidden in a hamburger with no visible cues.
- **Gesture-only critical actions**: Apple HIG explicitly warns gestures should be optional accelerators, never the only path to a core operation.
- **Overloaded top-level navigation**: too many peer destinations, ambiguous labels, or mixed navigation metaphors in one view (flat tabs + deep stacks confusing orientation together). Both Apple and Material discourage this.

## Touch target sizing (where platforms agree and diverge)

| Platform | Minimum | Notes |
|---|---|---|
| **iOS (Apple HIG)** | 44×44 pt | Long-standing baseline; visible glyph can be smaller than the tappable region if padding expands it |
| **Android/Material** | 48×48 dp | More conservative (~9mm); explicitly ties to accessibility guidance |
| **WCAG 2.2** | 24×24 CSS px minimum (2.5.8 Target Size), with exceptions | The accessibility floor — not the design target, the legal/usability minimum |

**Practical rule**: design to the larger platform standard (48dp/Material) whenever space allows, especially for icon-only controls and inline actions — never treat the smaller number as a target to hit, treat it as a floor.

## WCAG 2.2 — the real non-negotiable floor

These success criteria function as de facto rules for any modern site, not just legal compliance — WCAG 2.2 itself states this guidance generally improves usability, not only accessibility:

- **2.1.1 Keyboard** — everything must be operable by keyboard alone
- **2.4.7 / 2.4.11 / 2.4.12 / 2.4.13** — focus must be visible and never obscured
- **1.4.10 Reflow** — layout must reflow to 320 CSS px without introducing 2D scrolling
- **2.5.7 Dragging Movements** — never require drag-only interaction for a core task; provide a single-pointer alternative
- **2.5.8 Target Size (Minimum)** — 24×24 CSS px floor (design skill should treat platform minimums above as the real target, this as the absolute floor)
- **3.2.6 Consistent Help** — help mechanisms must appear in the same relative place across pages

## Information architecture

- **IA precedes UI.** Shopify Polaris explicitly states information architecture should work consistently across apps, editorial pages, desktop browsers, iOS, Android, chatbots, and retail surfaces — IA is a more stable layer than any individual visual component.
- **Content-out, not canvas-in.** A List Apart's framing (post-Marcotte): the interface should emerge from content, tasks, and context — not from a fixed screen template that content gets poured into.
- **Validate hierarchy before locking it.** Card sorting and tree testing are the evidence-based methods for confirming a navigation hierarchy matches users' actual mental model — don't guess IA from internal org-chart logic.
- **Mobile-first as a discipline, not just a screen size.** Luke Wroblewski's *Mobile First* (A Book Apart, 2011): the method is to design for the most constrained device first specifically *because* the constraint forces a real prioritization decision that a large screen lets you avoid — every feature has to justify its place when there's no room to hide indecision behind extra screen real estate. The payoff isn't just a smaller layout, it's a forcing function that produces a clearer hierarchy at every subsequent tier, because the core task was identified under pressure rather than assumed.
- **Four IA systems (Rosenfeld, Morville & Arango, *Information Architecture*, O'Reilly)** — apply this checklist explicitly when classifying an IA-heavy surface (editorial/docs/KB, application products in `surface-archetypes.md`):
  1. **Organization system** — how content is grouped (by topic, task, audience, chronology) — pick one primary scheme, don't blend several without a stated reason.
  2. **Labeling system** — the actual words used for links, headings, and categories — validate labels reflect users' language, not internal jargon.
  3. **Navigation system** — the structural means of moving through content (global, local, contextual) — distinct from the visual navigation *pattern* (tab bar vs. sidebar), this is the underlying system that pattern expresses.
  4. **Search system** — how findability works when browsing structure fails — a required system for content-heavy or catalog-heavy surfaces, not an optional add-on.

## Responsive mechanics — from Marcotte (2010) to today

- **Original premise (Ethan Marcotte, "Responsive Web Design," A List Apart, 2010)**: fluid grids + flexible images + media queries — one design system across screen sizes instead of disconnected device-specific experiences.
- **What evolved**: the same principle now operates at the component level. Container queries (`@container`, `cqw`/`cqi` units) let a component respond to its actual available space rather than only the viewport — critical for components reused across sidebars, modals, and split views where viewport width doesn't reflect local space.
- **Fluid typography**: `clamp()` with viewport or container units avoids the "jump" at fixed breakpoints; define min/preferred/max sizes tied to design tokens, not arbitrary numbers.

## Platform divergence (native) — when to follow it vs. build one system

- iOS leans on tab bars, navigation bars, sheets, split views; Android/Material leans on bottom navigation, side drawers/rails on larger form factors, and strong system back-gesture behavior.
- **Rule of thumb for cross-platform products**: the user's platform habits win for navigation *placement and affordance* (respect iOS tab-bar conventions on iPhone, Android back-gesture expectations on Android); the product's *content architecture and terminology* stay internally consistent regardless of platform.

## Scanning behavior (still-cited eye-tracking research)

- **F-pattern scanning** (NN/g): users scan poorly structured content in an F-shape — headings, lead sentences, and first words of lines carry disproportionate weight. Still cited, though understood as one scan pattern among several, not the only one.
- **Scroll and attention** (NN/g): people scroll further than early-2000s guidance assumed, but attention still drops sharply below the fold — put the most important content and actions high on the page regardless of scroll depth being "acceptable" now.

## Nielsen's 10 heuristics — still the baseline evaluation framework

Mobile-relevant subset: visibility of system status, match between system and real world, user control and freedom (undo/cancel paths), consistency and standards, recognition rather than recall, error prevention, flexibility and efficiency of use. Use as a final-pass audit, not a design generator.

## Where guidance is thin, contested, or inferred (flag, don't present as settled)

- Direct historical linkage from BlackBerry/Nokia-era constraints to today's specific frameworks is rarely spelled out explicitly in current documentation — the connection is inferred from continuity of principles (progressive disclosure, chunking) and the mobile-first discourse, not a direct citation trail.
- No widely documented AI "skill" or published framework was found that spans the full watch → phone → tablet → desktop → TV/video-wall range with a formal, structured adaptation taxonomy — this skill is filling a genuine gap, not reproducing an existing standard.
- Distance-based cognitive load research (handheld vs. desktop vs. living-room TV) is thin in primary literature; most platform guidance (Apple tvOS type defaults) implies the relationship through larger default type sizes rather than citing cognitive-psychology studies directly. Treat this as extrapolated, not settled research.
