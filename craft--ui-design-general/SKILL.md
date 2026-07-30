---
name: craft--ui-design-general
description: "Designs production-grade graphical interfaces for any platform — web, iOS, Android, macOS, Vision Pro, responsive, native, hybrid. Triggers on: UI design, build a page/screen/component, frontend design, app interface, dashboard, landing page, design a form/modal/nav, make it look good, redesign this, or any request to create visual interface code. Produces working code with a committed aesthetic direction, platform-aware patterns, and strict visual quality gates. One skill, one job: visual design execution. Pair with tool--ux-standards for compliance auditing."
compatibility: "No external tools required. Works with any frontend stack: HTML/CSS/JS, React, Next.js, Vue, Svelte, SwiftUI, React Native, Flutter, Tailwind, shadcn/ui, Jetpack Compose. Reads references/ for curated databases; falls back to training knowledge if files are absent."
classification: trusted-input
type: capability
last_reviewed: 2026-07-28
---

# Interface Design

You design and build graphical interfaces. Every screen you produce has a committed aesthetic direction, platform-appropriate patterns, and zero tolerance for generic AI output.

This skill covers the full visual design surface: pages, screens, components, layouts, dashboards, forms, navigation, modals, cards, data visualisation, onboarding flows — anything a user sees and touches.

**What this skill is NOT:**
- Design system architecture (tokens, governance, Figma org) — use `craft--design-system-and-figma-architecture`
- UX standards enforcement (WCAG audits, heuristic reviews) — use `tool--ux-standards`
- Brand voice or copywriting — use brand-voice skills

---

## How It Works

Every interface request follows three phases. No exceptions.

### Phase 1 — Design Brief (before any code)

Establish these before writing a single line:

| Dimension | What to decide |
|---|---|
| **Purpose** | What problem does this interface solve? Who uses it and in what context? |
| **Platform** | Web, iOS, Android, macOS, visionOS, responsive web, React Native, Flutter? Each has its own gravity. |
| **Aesthetic direction** | Choose ONE committed direction. See the Style System below. |
| **Typography** | Select a display + body pairing. See Typography Rules. |
| **Color** | Define a palette with CSS/design variables. See Color System. |
| **Layout model** | Grid structure, spacing scale, density level. |
| **Motion intent** | What moves, why, and how much. |
| **Constraints** | Framework, existing design system, performance budget, accessibility tier. |

If the user gives a vague request ("make it look good", "build a dashboard"), run the brief yourself — make opinionated choices, state them clearly, then build.

### Phase 2 — Design Execution

Build working, production-grade code that implements the brief. Every detail is intentional.

### Phase 3 — Quality Gate

Before delivering, verify against the Visual Quality Checklist (below). If anything fails, fix it before presenting.

---

## Style System

Choose one committed aesthetic direction per interface. Never blend randomly. The direction governs every subsequent decision.

### Core Directions

| Direction | Character | Best for |
|---|---|---|
| **Swiss Precision** | Grid-locked, functional, Helvetica-descended, restrained color | Enterprise tools, data platforms, professional dashboards |
| **Editorial** | Magazine-inspired, dramatic type scale, asymmetric columns, generous whitespace | Content platforms, blogs, portfolios, media |
| **Brutalist** | Raw, exposed structure, monospace, borders over shadows, high contrast | Developer tools, creative agencies, experimental products |
| **Soft Minimal** | Rounded corners, pastel palettes, airy spacing, subtle shadows | Consumer apps, wellness, social, onboarding |
| **Dark Luxury** | OLED blacks, gold/silver accents, thin weights, dramatic contrast | Premium products, finance, media, nightlife |
| **Glassmorphism** | Frosted layers, backdrop-blur, translucency, light refraction | Overlays, cards, modern dashboards, creative tools |
| **Neubrutalism** | Thick borders, solid shadows, bold primaries, playful geometry | Startups, SaaS marketing, creative tools |
| **Retro-Futurism** | Neon on dark, scan lines, monospace, terminal aesthetics | Dev tools, gaming, crypto, cybersecurity |
| **Organic** | Natural curves, earth tones, fluid shapes, hand-drawn elements | Sustainability, food, wellness, children |
| **Data-Dense** | Compact, information-rich, muted palette, small type, tight grids | Analytics, BI, trading, monitoring |
| **Bento** | Card-based grid, rounded modules, clear visual hierarchy | Feature showcases, portfolios, settings panels |
| **Art Deco** | Geometric patterns, gold accents, symmetry, serif display fonts | Luxury brands, events, premium finance |
| **Maximalist** | Layered, vibrant, mixed media, gradients, competing elements in controlled tension | Creative agencies, entertainment, fashion |

If the project demands something not listed, define it explicitly with the same structure: character, best-for, type direction, color mood, spacing philosophy.

### Platform-Specific Design Gravity

Every platform has native expectations. Respect them or consciously break them — never ignore them by accident.

**Web (Desktop)**
- Pointer-based interaction: hover states are mandatory
- 1200-1440px primary viewport, fluid below
- Scroll is the primary navigation mechanic
- Cursor changes communicate affordance

**Web (Responsive/Mobile)**
- Touch-first: 44px minimum tap targets
- Bottom-anchored primary actions (thumb zone)
- Swipe, pull-to-refresh as expected gestures
- Viewport units and container queries over fixed breakpoints
- No hover-dependent interactions

**iOS / macOS (Native)**
- SF Pro / SF Mono / New York type system
- Dynamic Type support (never fixed font sizes)
- Safe areas (notch, Dynamic Island, home indicator)
- Native components: tab bars, navigation bars, sheets, context menus
- Vibrancy and materials over solid backgrounds
- Haptic feedback vocabulary (light, medium, heavy, selection)

**Android (Native)**
- Material Design 3 component vocabulary
- Dynamic color (Material You) from wallpaper
- Edge-to-edge layout with system bar insets
- Predictive back gesture accommodation
- 48dp minimum touch targets

**visionOS**
- Glass material as the foundational surface
- Indirect gaze + pinch interaction model
- Volumetric depth and z-axis hierarchy
- Ornaments for supplementary controls
- Eyes-first design: no small targets, generous spacing

**React Native / Flutter (Cross-platform)**
- Platform-conditional styling (Platform.OS / Platform.select)
- Native navigation patterns per platform
- Respect safe areas on both platforms
- Performance-first: avoid opacity animations, use native driver

---

## Typography Rules

Typography is the single highest-leverage design decision. Get this right and mediocre everything else still looks designed. Get it wrong and nothing else saves it.

### Selection Principles

1. **Ban list.** Never use: Inter, Roboto, Arial, Helvetica Neue, system-ui as a design choice (system-ui is fine as a fallback stack, not as the aesthetic). Never converge on the same "safe" choice (Space Grotesk, Poppins, Outfit) across different projects.

2. **Pairing structure.** Every interface needs exactly two roles:
   - **Display**: Headlines, hero text, feature titles — characterful, bold, distinctive
   - **Body**: Running text, labels, descriptions — readable, neutral, workmanlike

3. **Scale.** Use a modular scale. Base size: 16px (web) / Dynamic Type body (iOS) / 14sp (Android). Scale ratio: 1.25 (minor third) for dense interfaces, 1.333 (perfect fourth) for editorial, 1.5 (perfect fifth) for dramatic.

4. **Weight range.** Use at most 3 weights per family. Weight creates hierarchy, not decoration.

5. **Line height.** Body text: 1.5-1.6. Headlines: 1.1-1.2. Never default to 1.0 or auto.

6. **Measure.** Body text: 45-75 characters per line. Never let paragraphs run full-width on desktop.

### Curated Pairings (reference, not exhaustive)

See `references/typography-pairings.md` for the full database. If the file is absent, select from training knowledge, ensuring the pairing has contrast (serif + sans, geometric + humanist, display + text).

---

## Color System

### Architecture

Every palette defines exactly these roles:

| Token | Purpose |
|---|---|
| `--background` | Page/screen canvas |
| `--surface` | Cards, panels, elevated containers |
| `--surface-raised` | Modals, popovers, floating elements |
| `--foreground` | Primary text |
| `--foreground-muted` | Secondary/supporting text |
| `--foreground-subtle` | Tertiary text, placeholders |
| `--border` | Default borders |
| `--border-strong` | Emphasized borders, dividers |
| `--accent` | Primary brand / action color |
| `--accent-foreground` | Text on accent backgrounds |
| `--destructive` | Error, danger, delete |
| `--success` | Confirmation, positive |
| `--warning` | Caution, attention |

### Rules

1. **Dominant color.** One color owns 60%+ of the surface. Sharp accents on a dominant base outperform evenly distributed palettes.

2. **Contrast.** Every foreground/background pair must meet WCAG AA (4.5:1 for text, 3:1 for large text and UI components). Check before shipping.

3. **Dark mode.** Never just invert. Dark mode needs its own considered palette: reduced contrast range, desaturated accents, elevated surfaces lighter than canvas.

4. **Semantic only.** Colors communicate meaning. Decorative color without semantic purpose is noise.

5. **CSS custom properties.** Always define as variables. Never hardcode hex values in component styles.

See `references/color-palettes.md` for curated industry-aligned palettes. If absent, construct palettes following these architectural rules.

---

## Layout & Spatial Composition

### Grid Systems

| Context | Recommendation |
|---|---|
| Marketing / editorial | 12-column with asymmetric content areas |
| App / dashboard | CSS Grid with named areas, sidebar + main |
| Mobile | Single column with section breaks, bottom nav |
| Data-dense | Compact grid, 8px base unit, minimal gutters |

### Spacing Scale

Use a geometric scale from a base unit:

- **Base**: 4px (compact) or 8px (standard)
- **Scale**: 4, 8, 12, 16, 24, 32, 48, 64, 96, 128
- **Rule**: Every margin, padding, and gap value comes from this scale. No magic numbers.

### Composition Principles

1. **Asymmetry over symmetry.** Centered, evenly-spaced layouts are the hallmark of undesigned UI. Offset, anchor, create visual tension.

2. **Density is a choice.** Data tools can be dense. Consumer apps need air. Neither is wrong — but mixing them in one interface is.

3. **Visual hierarchy through space.** More space around an element increases its importance. This is cheaper and more effective than making things bigger.

4. **Grid-breaking.** One element that intentionally breaks the grid creates a focal point. More than one creates chaos.

5. **Negative space is structure.** Empty space is not wasted space. It groups, separates, and directs attention.

---

## Motion & Interaction Design

### Principles

1. **Motion communicates.** Every animation answers a question: Where did this come from? Where is it going? What changed? If an animation doesn't answer a question, delete it.

2. **One hero moment.** A page load with staggered reveals creates more impact than scattered micro-interactions everywhere. Focus the motion budget.

3. **Duration scale:**
   - Micro-interactions (button, toggle): 100-200ms
   - Transitions (panel slide, card flip): 200-400ms
   - Page transitions: 300-500ms
   - Staggered reveals: 50-100ms delay between items

4. **Easing:**
   - Enter: `ease-out` (decelerate into rest)
   - Exit: `ease-in` (accelerate away)
   - Move: `ease-in-out` (smooth both ends)
   - Spring physics for natural feel (React Native, SwiftUI, Motion)

5. **Reduced motion.** Always respect `prefers-reduced-motion`. Replace animations with instant state changes or opacity-only crossfades. Non-negotiable.

6. **CSS-first.** Prefer CSS transitions and animations over JS. Use Motion (Framer Motion), GSAP, or native Animated APIs only when CSS cannot achieve the effect.

### Interaction States

Every interactive element must define all applicable states:

| State | Required for |
|---|---|
| Default | Everything |
| Hover | Pointer devices (web, macOS) |
| Active/Pressed | Everything |
| Focus-visible | Everything (keyboard navigation) |
| Disabled | Anything that can be disabled |
| Loading | Async actions |
| Error | Form inputs, submissions |
| Selected/Active | Toggles, tabs, nav items |
| Dragging | Draggable elements |

---

## Backgrounds & Visual Details

The background is not an afterthought. It creates atmosphere.

### Techniques (match to aesthetic direction)

- **Gradient meshes** — Organic, modern. 3-4 color stops with radial gradients at offset positions.
- **Noise/grain** — Adds texture and warmth. SVG filter or CSS `url()` with low opacity (0.03-0.08).
- **Geometric patterns** — Subtle repeating SVG. Works for tech, data, enterprise.
- **Dot grids** — Swiss, editorial. Radial-gradient repeating pattern.
- **Layered transparencies** — Glassmorphism base. Multiple surfaces at different opacities.
- **Dramatic shadows** — Depth and elevation. Layered box-shadows (2-3 shadows per element).
- **Decorative borders** — Brutalist, neubrutalist. Thick, offset, colored.

### Anti-Patterns (never do these)

- Solid white or solid #000 backgrounds with no texture or depth
- Purple gradients on white (the "AI startup" cliche)
- Stock photo hero backgrounds with text overlay
- Gradient borders on everything
- Glow effects on every interactive element

---

## Visual Quality Checklist

Run this before delivering any interface. Every item must pass.

### Aesthetic Coherence
- [ ] Single committed aesthetic direction — no style mixing
- [ ] Typography pairing is intentional, not default
- [ ] Color palette uses defined tokens, no hardcoded values
- [ ] Spacing uses the scale exclusively — no magic numbers
- [ ] Visual hierarchy is clear without reading any text

### Platform Fidelity
- [ ] Follows platform conventions (or consciously breaks them with justification)
- [ ] Touch targets meet platform minimums (44pt iOS, 48dp Android, 44px web)
- [ ] Safe areas respected (notch, home indicator, system bars)
- [ ] Text sizing follows platform model (Dynamic Type, sp, rem)

### Interaction Completeness
- [ ] All interactive elements have hover, active, focus-visible, and disabled states
- [ ] Loading states exist for all async operations
- [ ] Error states exist for all form inputs
- [ ] Empty states designed (not just "no data")
- [ ] Transitions respect `prefers-reduced-motion`

### Visual Polish
- [ ] No orphaned text (single words on their own line in headlines)
- [ ] Icons are consistent in style, weight, and size
- [ ] Images have proper aspect ratios and loading behavior
- [ ] Shadows and elevations are consistent and systematic
- [ ] Border radii are consistent (one radius scale, not random values)

### Code Quality
- [ ] Semantic HTML structure (not div soup)
- [ ] CSS custom properties for all design tokens
- [ ] Responsive without horizontal scroll at any viewport
- [ ] No unused CSS or dead component code
- [ ] Component boundaries are logical (one component = one visual unit)

---

## Anti-AI-Slop Enforcement

These patterns are banned. They signal AI-generated, undesigned output.

### Banned Defaults
- Inter, Roboto, Arial, or system fonts as design choices
- `#6366f1` (indigo-500) or any default Tailwind purple as a primary color
- Card grids with identical padding, identical radius, identical shadow, identical spacing
- Hero sections with centered text + gradient background + "Get Started" button
- Three-column feature grids with icon + title + description
- Testimonial carousels with circular avatar photos
- "Built with love" footers

### Banned Behaviors
- Using a CSS framework's defaults without customisation
- Copying a template without transforming the aesthetic
- Applying the same visual treatment to every project
- Defaulting to light theme without considering the context
- Using emoji as UI icons

### Required Differentiation
Every interface must have at least ONE element that a viewer would remember. A distinctive type choice. An unexpected layout. A clever interaction. A bold color move. Something that says "a designer made this decision" — not "a model produced this output."

### Why this matters beyond taste: the Aesthetic-Usability Effect
Users perceive aesthetically pleasing designs as *more usable*, independent of whether they actually are (documented in Kurosu & Kashimura's 1995 study, cited widely in HCI research including Yablonski's *Laws of UX*). This cuts both ways: polish buys real goodwill and forgiveness for minor friction — but it also means a beautiful interface can mask a genuinely broken flow, delaying the discovery of usability problems until real damage is done. Treat visual quality as a multiplier on trust, not a substitute for the interaction actually working — the Quality Gate's other sections (interaction states, platform fidelity) are not optional just because the aesthetic direction is strong.

---

## Output Format

Deliver working code in the user's requested stack. If no stack is specified, default to:
- **Web**: HTML + CSS + vanilla JS (most portable)
- **React**: TSX + Tailwind (with custom theme) + shadcn/ui foundation
- **iOS**: SwiftUI
- **Android**: Jetpack Compose + Material 3
- **Cross-platform**: React Native + NativeWind or Flutter

Always include:
1. **Design brief** (3-5 lines stating the aesthetic direction, type choices, color palette, and layout model)
2. **Working code** (complete, runnable, no placeholders)
3. **Token definitions** (CSS variables, Tailwind config, or platform equivalent)

Never include:
- Lorem ipsum (use realistic placeholder content)
- Placeholder images with `via.placeholder.com` (use solid color blocks or SVG illustrations)
- Comments explaining what the code does (the code explains itself)
- Multiple options to choose from (commit to one direction and execute it)
