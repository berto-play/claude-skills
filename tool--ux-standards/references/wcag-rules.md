# WCAG 2.2 Quick Reference — Audit Companion

This file supplements the rule table in SKILL.md with implementation details, common failure patterns, and test methods for each criterion.

## Testing Methodology

For each criterion, test using this sequence:
1. **Code inspection** — Read the markup/styles for structural compliance
2. **Keyboard-only navigation** — Tab through the entire interface without a mouse
3. **Screen reader check** — Verify announcements match visible content
4. **Zoom test** — Scale to 200% text zoom and 400% page zoom
5. **Reflow test** — Resize to 320px viewport width

---

## Perceivable

### 1.1.1 Non-text Content (Level A)

**Common failures:**
- `<img>` without `alt` attribute
- Icon buttons without `aria-label` (e.g., `<button><svg>...</svg></button>`)
- CSS background images that convey information
- `<input type="image">` without `alt`
- Charts/graphs without text description
- CAPTCHAs without alternative

**Fix patterns:**
```html
<!-- Informative image -->
<img src="chart.png" alt="Revenue grew 23% in Q4 2025" />

<!-- Decorative image -->
<img src="divider.svg" alt="" role="presentation" />

<!-- Icon button -->
<button aria-label="Close dialog">
  <svg aria-hidden="true">...</svg>
</button>

<!-- Complex image -->
<figure>
  <img src="flowchart.png" alt="User onboarding flow" />
  <figcaption>Detailed description of each step...</figcaption>
</figure>
```

### 1.3.1 Info and Relationships (Level A)

**Common failures:**
- Skipped heading levels (`<h1>` then `<h3>`)
- Visual tables built with `<div>` grids
- Bold text used for headings instead of heading elements
- Lists rendered as `<div>` chains
- Form groups without `<fieldset>` and `<legend>`
- No landmark regions (`<nav>`, `<main>`, `<aside>`)

**Heading order rule:** One `<h1>` per page. Levels increase by 1 only. No skipping. Nested sections nest headings.

### 1.4.3 Contrast Minimum (Level AA)

**Required ratios:**
| Element | Minimum ratio |
|---|---|
| Normal text (<18pt / <14pt bold) | 4.5:1 |
| Large text (>=18pt / >=14pt bold) | 3:1 |
| UI components (borders, icons, form outlines) | 3:1 |
| Focus indicators | 3:1 against adjacent color |

**Common failures:**
- Light grey text on white (`#999` on `#fff` = 2.85:1, FAIL)
- Placeholder text with insufficient contrast
- Disabled elements that still need to be readable
- Text over images without overlay/scrim
- Accent colors that pass on light but fail on dark mode (or vice versa)

**Test method:** Check every unique foreground/background combination. Tools: WebAIM Contrast Checker, browser DevTools accessibility panel.

### 1.4.10 Reflow (Level AA)

**The 320px rule:** At 1280px viewport width zoomed to 400%, content equivalent to 320px wide. No horizontal scrollbar.

**Exempt content:** Data tables, maps, code blocks, videos — anything that requires two-dimensional layout.

**Common failures:**
- Fixed-width containers (`width: 800px`)
- Horizontal scrolling nav that doesn't wrap
- Side-by-side layouts that don't stack
- Absolute positioning that overlaps at narrow widths
- `overflow: hidden` hiding content instead of reflowing

### 1.4.11 Non-text Contrast (Level AA)

**What needs 3:1 contrast:**
- Input field borders (against background)
- Custom checkboxes, radio buttons, toggles
- Icons that convey meaning
- Chart lines, bars, segments
- Focus indicators
- Button borders (if border is the affordance)

**What's exempt:**
- Inactive/disabled components
- Components styled entirely by the browser (default `<select>`)
- Logos and decorative graphics

### 1.4.13 Content on Hover or Focus (Level AA)

**The three rules for tooltips/popovers:**
1. **Dismissible** — Escape key closes without moving focus
2. **Hoverable** — User can move mouse from trigger to tooltip without it closing
3. **Persistent** — Stays visible until user dismisses, moves focus, or hovers away

**Common failures:**
- Tooltip disappears when mouse moves toward it
- No keyboard dismissal
- Content disappears on a timer
- Tooltip obscures the trigger element

---

## Operable

### 2.1.1 Keyboard (Level A)

**Test method:** Unplug mouse. Tab through entire interface. Every interactive element must be:
- Reachable via Tab/Shift+Tab
- Activatable via Enter or Space
- Operable via arrow keys (for widget patterns)

**Common failures:**
- `<div onclick>` with no `tabindex="0"` or `role="button"` or `onkeydown`
- Custom dropdown without arrow key navigation
- Drag-and-drop without keyboard alternative
- Slider without arrow key support
- Canvas-based interactions with no keyboard path

### 2.4.7 Focus Visible (Level AA)

**Minimum requirements:**
- Focus indicator is visible on every focusable element
- 2px minimum thickness
- 3:1 contrast ratio against adjacent color
- `outline: none` or `outline: 0` without custom replacement is BANNED

**Fix pattern:**
```css
:focus-visible {
  outline: 2px solid var(--accent);
  outline-offset: 2px;
}
```

### 2.5.5 Target Size (Level AA — new in WCAG 2.2)

**Minimum: 24x24 CSS pixels** for all interactive targets.

**Exceptions:**
- Inline text links within a sentence
- Targets where the size is determined by the user agent (default `<select>`)
- Targets that are essential (a specific pixel on a map)

**Calculation:** The target area includes padding. A 16x16 icon with 4px padding on all sides = 24x24 total. Passes.

---

## Understandable

### 3.3.1 Error Identification (Level A)

**Required for every form error:**
1. Error text (not just color change)
2. Which field has the error
3. What the error is

**Fix pattern:**
```html
<label for="email">Email address</label>
<input id="email" aria-describedby="email-error" aria-invalid="true" />
<p id="email-error" role="alert">Enter a valid email address (e.g., name@example.com)</p>
```

### 3.3.2 Labels or Instructions (Level A)

**Rule:** Every form input has a visible `<label>` or `aria-label`.

**Placeholder is NOT a label.** Placeholders disappear on input, are low-contrast, and are not announced consistently by screen readers.

**Required fields:** Mark with text ("required") or `aria-required="true"`. Asterisk (*) alone is insufficient without a legend explaining it.

---

## Robust

### 4.1.2 Name, Role, Value (Level A)

**Use semantic HTML first:**
| Need | HTML solution | ARIA fallback |
|---|---|---|
| Button | `<button>` | `<div role="button" tabindex="0">` |
| Link | `<a href>` | `<span role="link" tabindex="0">` |
| Dialog | `<dialog>` | `<div role="dialog" aria-modal="true">` |
| Tab | — | `role="tablist"`, `role="tab"`, `role="tabpanel"` |
| Accordion | `<details>` / `<summary>` | `role="region"` + `aria-expanded` |

**First rule of ARIA:** Don't use ARIA if you can use semantic HTML.

### 4.1.3 Status Messages (Level A)

**Dynamic content must be announced:**
```html
<!-- Toast notification -->
<div role="status" aria-live="polite">Item added to cart</div>

<!-- Error summary -->
<div role="alert">3 errors found. Please review the form.</div>

<!-- Live counter -->
<span aria-live="polite" aria-atomic="true">3 items remaining</span>
```

**`aria-live` values:**
- `polite` — Announced at next pause (notifications, status updates)
- `assertive` — Announced immediately (errors, urgent alerts)
