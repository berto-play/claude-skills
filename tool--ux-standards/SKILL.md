---
name: tool--ux-standards
description: "Audits and enforces UX standards across any interface — web, iOS, Android, macOS, visionOS, responsive, native, hybrid. Triggers on: accessibility check, WCAG audit, UX review, heuristic evaluation, check my UI, review this component, is this accessible, usability review, compliance check, or when paired with craft--ui-design-general as a pre-delivery gate. Covers WCAG 2.2, Nielsen's heuristics, platform HIGs, responsive design rules, performance guidelines, and cognitive accessibility. Outputs severity-scored findings with exact fix instructions."
compatibility: "No external tools required. Reads references/ for the complete rule database. Falls back to training knowledge if files are absent, labelled as 'General standard' rather than 'Rule database'."
classification: trusted-input
last_reviewed: 2026-07-28
---

# UX Standards

You audit interfaces against established UX standards. You find what's wrong, score its severity, and provide exact fixes.

This skill is the compliance and quality layer. It does not make aesthetic choices (that's `craft--ui-design-general`) or architect design systems (that's `craft--design-system-and-figma-architecture`). It verifies that interfaces meet measurable, testable standards.

**What this skill covers:**
- WCAG 2.2 accessibility (Level A, AA, and AAA criteria)
- Nielsen's 10 usability heuristics
- Platform-specific HIG compliance (Apple, Material, Web)
- Responsive design and viewport rules
- Performance and loading UX
- Cognitive accessibility
- Form design standards
- Navigation and wayfinding
- Touch and pointer interaction rules
- Motion and animation safety
- Dark mode and theming standards
- Internationalisation readiness

---

## How It Works

### Input

The user provides one or more of:
- Code (HTML, CSS, JS, JSX, TSX, SwiftUI, Compose, Flutter)
- A screenshot or description of an interface
- A URL to review
- A component, page, or flow to audit

### Output

A structured audit report with severity-scored findings. Every finding includes:

1. **Rule ID** — From the rule database (e.g., `WCAG-1.4.3`, `NIE-04`, `RESP-07`)
2. **Severity** — Critical / Major / Minor / Advisory
3. **Location** — File:line or element description
4. **Issue** — What's wrong, in one sentence
5. **Standard** — Which standard this violates
6. **Fix** — Exact code or design change to resolve it

### Severity Scale

| Level | Meaning | Action |
|---|---|---|
| **Critical** | Blocks users entirely. Legal liability. Screen reader cannot parse. Keyboard trap. Missing form labels. | Must fix before shipping. |
| **Major** | Significant usability impact. Poor contrast. Missing focus states. Broken responsive layout. | Should fix before shipping. |
| **Minor** | Suboptimal but functional. Inconsistent spacing. Non-ideal touch targets. Missing hover state. | Fix in next iteration. |
| **Advisory** | Best practice recommendation. Could improve but not broken. | Consider for polish pass. |

---

## Audit Modes

| User says | Mode | Scope |
|---|---|---|
| "Check this for accessibility" | **WCAG Audit** | Full WCAG 2.2 AA scan |
| "Review this UI" | **Full Audit** | All standards: WCAG + heuristics + platform + responsive + performance |
| "Is this usable?" | **Heuristic Review** | Nielsen's 10 heuristics focused |
| "Check this before shipping" | **Pre-Delivery Gate** | Full audit with pass/fail verdict |
| "Review for [platform]" | **Platform Audit** | Platform-specific HIG compliance |
| "Quick check" | **Spot Check** | Top 10 most common issues only |

---

## Rule Database

### Section 1 — WCAG 2.2 Accessibility

Full criteria reference in `references/wcag-rules.md`. Key rules enforced:

#### Perceivable

| Rule ID | Criterion | Requirement |
|---|---|---|
| WCAG-1.1.1 | Non-text Content | Every `<img>`, `<svg>`, icon, and visual element has text alternative. Decorative images use `alt=""` or `role="presentation"`. |
| WCAG-1.3.1 | Info and Relationships | Structure conveyed through headings (`h1`-`h6` in order), lists, tables, landmarks (`nav`, `main`, `aside`, `footer`). Not through visual formatting alone. |
| WCAG-1.3.2 | Meaningful Sequence | DOM order matches visual order. CSS `order`, `flex-direction: row-reverse`, absolute positioning must not break reading sequence. |
| WCAG-1.3.4 | Orientation | Content not locked to portrait or landscape unless essential (e.g., piano app). |
| WCAG-1.3.5 | Identify Input Purpose | Form inputs use `autocomplete` attributes for personal data (name, email, tel, address). |
| WCAG-1.4.1 | Use of Color | Color is never the sole means of conveying information. Links have underlines OR non-color indicator. Error states use icon + text, not just red. |
| WCAG-1.4.3 | Contrast Minimum | Text: 4.5:1 ratio. Large text (18pt+ or 14pt+ bold): 3:1. Check every foreground/background pair. |
| WCAG-1.4.4 | Resize Text | Text scales to 200% without loss of content or function. No `overflow: hidden` on text containers with fixed heights. |
| WCAG-1.4.10 | Reflow | Content reflows at 320px width (400% zoom) without horizontal scrolling. No two-dimensional scrolling except for content that requires it (data tables, maps). |
| WCAG-1.4.11 | Non-text Contrast | UI components and graphical objects: 3:1 contrast against adjacent colours. Borders, icons, focus indicators, chart elements. |
| WCAG-1.4.12 | Text Spacing | Content remains functional when user overrides: line-height to 1.5x, paragraph spacing to 2x, letter-spacing to 0.12em, word-spacing to 0.16em. |
| WCAG-1.4.13 | Content on Hover or Focus | Tooltips/popovers: dismissible (Escape), hoverable (mouse can move to tooltip), persistent (stays until dismissed). |

#### Operable

| Rule ID | Criterion | Requirement |
|---|---|---|
| WCAG-2.1.1 | Keyboard | Every interactive element reachable and operable by keyboard alone. No keyboard traps. Custom widgets need full keyboard support. |
| WCAG-2.1.2 | No Keyboard Trap | Focus can always move away from any component using Tab, Shift+Tab, Escape, or arrow keys. Modal focus traps must have an exit. |
| WCAG-2.4.1 | Bypass Blocks | Skip-to-content link present and functional. Landmark regions defined. |
| WCAG-2.4.3 | Focus Order | Tab order follows logical reading sequence. No focus jumping across the page. `tabindex` values > 0 are banned. |
| WCAG-2.4.6 | Headings and Labels | Headings describe content. Labels describe purpose. No generic labels ("click here", "read more" without context). |
| WCAG-2.4.7 | Focus Visible | Every focusable element has a visible focus indicator. Minimum 2px solid outline. `outline: none` without replacement is banned. |
| WCAG-2.4.11 | Focus Not Obscured | Focused element is not entirely hidden by sticky headers, footers, modals, or other overlapping content. At least partially visible. |
| WCAG-2.5.5 | Target Size | Interactive targets: minimum 24x24 CSS px (Level AA). Recommended 44x44px. Inline text links exempt. |
| WCAG-2.5.8 | Target Size (Enhanced) | Minimum 44x44 CSS px for all targets (Level AAA). |

#### Understandable

| Rule ID | Criterion | Requirement |
|---|---|---|
| WCAG-3.1.1 | Language of Page | `<html lang="xx">` present and correct. |
| WCAG-3.2.1 | On Focus | Focus alone does not trigger a change of context (no auto-submit, no navigation on focus). |
| WCAG-3.2.2 | On Input | Changing a form control does not automatically trigger a change of context unless user is warned beforehand. |
| WCAG-3.3.1 | Error Identification | Errors identified in text. Which field has the error and what's wrong. Not just a red border. |
| WCAG-3.3.2 | Labels or Instructions | Form fields have visible labels. Placeholder text is not a substitute for labels. |
| WCAG-3.3.3 | Error Suggestion | When an error is detected, suggest a correction if known (e.g., "Did you mean .com?"). |
| WCAG-3.3.8 | Accessible Authentication | No cognitive function test (CAPTCHA) as the sole authentication method. Provide alternatives. |

#### Robust

| Rule ID | Criterion | Requirement |
|---|---|---|
| WCAG-4.1.2 | Name, Role, Value | Custom components expose name, role, and state to assistive tech. Use ARIA only when semantic HTML is insufficient. |
| WCAG-4.1.3 | Status Messages | Dynamic content updates announced by screen readers. Use `role="status"`, `role="alert"`, or `aria-live` regions. |

---

### Section 2 — Nielsen's 10 Usability Heuristics

Full reference in `references/heuristic-rules.md`.

| Rule ID | Heuristic | What to check |
|---|---|---|
| NIE-01 | Visibility of System Status | Loading indicators for all async operations. Progress bars for multi-step processes. Confirmation messages for completed actions. Timestamps for data freshness. |
| NIE-02 | Match Between System and Real World | Labels use user's vocabulary, not internal jargon. Icons match real-world metaphors. Flows follow real-world task order. No developer-facing error messages. |
| NIE-03 | User Control and Freedom | Undo available for destructive actions. Cancel buttons on all modals/forms. Back navigation always works. Exit path visible from every state. |
| NIE-04 | Consistency and Standards | Same action, same appearance, everywhere. Platform conventions followed. Terminology consistent across all screens. Icon meanings don't change. |
| NIE-05 | Error Prevention | Destructive actions require confirmation. Form validation happens inline and before submission. Autocomplete reduces input errors. Constraints prevent invalid states. |
| NIE-06 | Recognition Rather Than Recall | Options visible, not hidden behind memory. Recent items shown. Search with suggestions. No blank states — always show guidance. |
| NIE-07 | Flexibility and Efficiency | Power users have shortcuts. Experts can skip tutorials. Settings remember preferences. Batch operations for repetitive tasks. |
| NIE-08 | Aesthetic and Minimalist Design | Every element earns its space. No decoration without function. Information hierarchy clear. No competing calls-to-action. |
| NIE-09 | Error Recovery | Error messages: (1) identify what went wrong, (2) in plain language, (3) suggest a fix. Never: "Error 500", "Something went wrong", or raw stack traces. |
| NIE-10 | Help and Documentation | Contextual help where needed. Tooltips for complex features. Searchable help if product is complex. Onboarding for first-time features. |

---

### Section 3 — Platform HIG Compliance

#### Apple Platforms (iOS, macOS, visionOS)

| Rule ID | Rule | Requirement |
|---|---|---|
| APL-01 | Safe Areas | Content respects safe area insets. No content behind notch, Dynamic Island, or home indicator. Use `safeAreaInset` in SwiftUI, `safe-area-inset-*` in CSS. |
| APL-02 | Dynamic Type | All text supports Dynamic Type sizes from xSmall to AX5. No truncation at large sizes — layout reflows. |
| APL-03 | Navigation | Use standard navigation patterns: NavigationStack (push), TabView (tabs), sheet (modals). No custom navigation that breaks swipe-to-go-back. |
| APL-04 | Tab Bar | Maximum 5 tabs. Icons + labels always. No text-only tabs. Active state clearly distinguished. |
| APL-05 | Dark Mode | Full dark mode support. Use semantic colors (`.label`, `.secondaryLabel`, `.systemBackground`). No hardcoded colors. |
| APL-06 | Haptics | Use system haptics for feedback: `.selection` for picks, `.impact` for confirmations, `.notification` for outcomes. |
| APL-07 | VoiceOver | Every interactive element has an accessibility label. Images have descriptions. Custom views implement accessibility traits. |
| APL-08 | Materials & Vibrancy | Use system materials (`.ultraThinMaterial`, `.regularMaterial`) for translucent surfaces instead of custom opacity values. |

#### Material Design (Android)

| Rule ID | Rule | Requirement |
|---|---|---|
| MAT-01 | Edge-to-edge | Content draws behind system bars. Use `WindowCompat.setDecorFitsSystemWindows(window, false)` and handle insets. |
| MAT-02 | Dynamic Color | Support Material You dynamic theming from wallpaper. Define fallback static palette. |
| MAT-03 | Touch Targets | Minimum 48x48dp for all touch targets. Minimum 8dp spacing between adjacent targets. |
| MAT-04 | Navigation | Bottom navigation for 3-5 top-level destinations. Navigation drawer for 6+. No combined bottom nav + drawer. |
| MAT-05 | Typography Scale | Use Material 3 type scale: displayLarge through labelSmall. Map custom fonts to these roles. |
| MAT-06 | Predictive Back | Support predictive back gesture. No custom back handling that blocks system gesture. |
| MAT-07 | Elevation | Use tonal elevation (surface color change), not shadow elevation, for Material 3. |

#### Web Platform

| Rule ID | Rule | Requirement |
|---|---|---|
| WEB-01 | Semantic HTML | Use `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<header>`, `<footer>`. Not `<div>` for everything. |
| WEB-02 | Forms | Every `<input>` has a visible `<label>`. `placeholder` is not a label replacement. Required fields marked. Error messages linked to fields with `aria-describedby`. |
| WEB-03 | Links vs Buttons | `<a>` navigates. `<button>` performs an action. Never `<div onclick>` or `<span onclick>`. |
| WEB-04 | Skip Link | First focusable element is a "Skip to main content" link. Hidden until focused. |
| WEB-05 | Focus Management | Focus moves to new content when: modal opens, page navigates (SPA), drawer opens. Focus returns when: modal closes, drawer closes. |
| WEB-06 | Scroll | No content lost to `overflow: hidden` on containers without scroll. No horizontal scroll at any viewport above 320px. |
| WEB-07 | Touch vs Pointer | No hover-dependent functionality on touch devices. Touch targets 44px minimum. No double-tap-to-zoom conflicts. |

---

### Section 4 — Responsive Design Rules

| Rule ID | Rule | Requirement |
|---|---|---|
| RESP-01 | Breakpoints | Content-driven breakpoints, not device-driven. Test at: 320px, 375px, 428px, 768px, 1024px, 1280px, 1440px, 1920px. |
| RESP-02 | No Horizontal Scroll | At 320px viewport width, no horizontal scroll. Content reflows to single column. |
| RESP-03 | Images | Use `srcset` and `sizes` for responsive images. Never serve desktop images to mobile. Use `aspect-ratio` to prevent layout shift. |
| RESP-04 | Typography Scale | Type scales down on mobile. Headlines reduce by 1-2 steps. Body text never below 16px on mobile (prevents iOS zoom). |
| RESP-05 | Touch Targets | 44px minimum on touch viewports. Increase spacing between adjacent targets. |
| RESP-06 | Navigation | Desktop: horizontal nav or sidebar. Tablet: collapsible sidebar. Mobile: bottom tab bar or hamburger menu. |
| RESP-07 | Tables | Data tables get horizontal scroll container on mobile. Or reformat to card layout. Never squish columns to unreadable widths. |
| RESP-08 | Container Queries | Prefer container queries over media queries for component-level responsiveness. Components should respond to their container, not the viewport. |
| RESP-09 | Fluid Typography | Use `clamp()` for font sizes that scale between viewports: `clamp(1rem, 2.5vw, 1.5rem)`. No abrupt size jumps at breakpoints. |
| RESP-10 | Testing | Test with real content, not lorem ipsum. Short and long content. Test with text zoom at 200%. Test with system font size overrides. |

---

### Section 5 — Performance UX

| Rule ID | Rule | Requirement |
|---|---|---|
| PERF-01 | Loading States | Every async operation has a loading indicator. Skeleton screens preferred over spinners for content areas. |
| PERF-02 | Layout Shift | No cumulative layout shift from lazy-loaded content. Reserve space with `aspect-ratio`, `min-height`, or skeleton placeholders. |
| PERF-03 | Interaction Responsiveness | UI responds to input within 100ms. Visual feedback within 50ms. Complete state change within 500ms or show progress. |
| PERF-04 | Lazy Loading | Off-screen images and heavy content use `loading="lazy"`. Below-fold components use dynamic imports. |
| PERF-05 | Font Loading | `font-display: swap` for all web fonts. Preload primary font. Fallback stack with matching metrics. |
| PERF-06 | Image Formats | Use WebP or AVIF with fallbacks. Compress images. Serve responsive sizes. |
| PERF-07 | Perceived Speed | Show content progressively. Optimistic UI for user actions (show success before server confirms). Skeleton screens during data fetch. |

---

### Section 6 — Cognitive Accessibility

| Rule ID | Rule | Requirement |
|---|---|---|
| COG-01 | Cognitive Load | One primary action per screen/section. Clear visual hierarchy. No competing CTAs. |
| COG-02 | Plain Language | Interface text at 8th-grade reading level or below. No jargon without explanation. Short sentences. |
| COG-03 | Consistent Layout | Navigation and key elements stay in the same position across screens. No layout surprises. |
| COG-04 | Timeout Warnings | If a session times out, warn before it happens. Allow extension. Save user's progress. |
| COG-05 | Error Recovery | Never lose user data on error. Auto-save form progress. Allow undo. |
| COG-06 | Instructions | Complex interactions have visible instructions. Don't rely on user remembering a tutorial. |
| COG-07 | Animation Safety | No auto-playing animation longer than 5 seconds without pause control. No flashing content (3 flashes/second max). Respect `prefers-reduced-motion`. |

---

### Section 7 — Form Design Standards

| Rule ID | Rule | Requirement |
|---|---|---|
| FORM-01 | Labels | Every input has a visible, persistent label. Labels above inputs (not inline, not beside). |
| FORM-02 | Placeholder Text | Placeholder is a hint, not a label. Disappears on focus — cannot be the only instruction. |
| FORM-03 | Required Fields | Mark required fields. Either mark required (preferred) or mark optional. Never leave ambiguous. |
| FORM-04 | Validation Timing | Validate on blur (field exit), not on every keystroke. Show errors inline next to the field. |
| FORM-05 | Error Messages | State what's wrong + how to fix it. "Email must include @" not "Invalid input". |
| FORM-06 | Autocomplete | Use `autocomplete` attributes: `name`, `email`, `tel`, `address-line1`, `cc-number`, etc. Browsers and password managers need these. |
| FORM-07 | Input Types | Use correct input types: `type="email"`, `type="tel"`, `type="url"`, `type="number"`, `type="date"`. Mobile keyboards depend on this. |
| FORM-08 | Multi-step Forms | Show progress indicator. Allow back navigation. Save progress between steps. Show summary before final submission. |
| FORM-09 | Submit Button | Button label describes the action: "Create Account", "Send Message", "Save Changes". Never just "Submit". |

---

### Section 8 — Navigation & Wayfinding

| Rule ID | Rule | Requirement |
|---|---|---|
| NAV-01 | Current Location | User always knows where they are. Active nav item highlighted. Breadcrumbs for deep hierarchies. |
| NAV-02 | Predictable Navigation | Navigation stays consistent across pages. No items appearing/disappearing. No re-ordering. |
| NAV-03 | Breadcrumbs | Present for hierarchies deeper than 2 levels. Clickable ancestors. Current page shown but not linked. |
| NAV-04 | Back Navigation | Browser back button works correctly in SPAs. State preserved on back. No broken history entries. |
| NAV-05 | Search | Present for content-heavy apps (>20 pages or >50 items). Autocomplete suggestions. Tolerant of typos. |
| NAV-06 | 404 / Empty States | Helpful, not dead-end. Suggest alternatives. Link to home or search. Never a raw error page. |

---

### Section 9 — Dark Mode & Theming

| Rule ID | Rule | Requirement |
|---|---|---|
| THEME-01 | System Preference | Respect `prefers-color-scheme`. Default to system setting. Allow manual override. |
| THEME-02 | No Inversion | Dark mode is not inverted light mode. Separate palette with: reduced contrast range, desaturated accents, elevated surfaces lighter than canvas. |
| THEME-03 | Images | Images designed for light backgrounds need dark-mode variants or bordered containers. No bright images floating on dark backgrounds. |
| THEME-04 | Shadows | Shadows near-invisible on dark backgrounds. Use border or subtle glow for elevation cues instead. |
| THEME-05 | Semantic Tokens | All colors defined as semantic tokens, not raw values. `--text-primary` not `#333333`. Theme switch changes tokens, not individual styles. |

---

## Audit Report Format

Output every audit as a structured report:

```
## UX Standards Audit — [Component/Page Name]

**Mode:** [Full Audit / WCAG / Heuristic / Platform / Pre-Delivery]
**Platform:** [Web / iOS / Android / Cross-platform]
**Date:** [Current date]

### Summary
- Critical: [count]
- Major: [count]
- Minor: [count]
- Advisory: [count]
- **Verdict:** [PASS / FAIL — fails if any Critical or Major issues]

### Findings

#### [Severity] — [Rule ID]: [One-line issue]
**Location:** [file:line or element description]
**Standard:** [Full standard name]
**Issue:** [What's wrong]
**Fix:**
[Exact code change or design instruction]

---
[Repeat for each finding]
```

### Pre-Delivery Gate

When running as a pre-delivery gate (paired with `craft--ui-design-general`):

- Run the Full Audit mode
- **PASS** = zero Critical, zero Major findings
- **FAIL** = any Critical or Major finding present
- On FAIL: list all Critical and Major findings with fixes. Do not deliver the interface until resolved.
- On PASS: state "Pre-delivery gate: PASSED" and list any Minor/Advisory items for optional polish.

---

## What This Skill Does NOT Do

- **Design interfaces** — Use `craft--ui-design-general` for visual design execution
- **Architect design systems** — Use `craft--design-system-and-figma-architecture`
- **Write copy** — Use brand-voice skills
- **Make aesthetic judgments** — This skill checks standards, not taste
- **Replace automated testing** — This is a design review, not axe-core or Lighthouse. Use automated tools alongside this skill for complete coverage.
