# The Standards of Judgment

Source: THE MANIFESTO OF BALANCED CREATION. Every work passes all seven, in order, or it does not ship. **A failure is a defect, not a difference of taste. Fail-closed: unknown counts as failed.**

## 1. MEANING
**Question:** What is the idea? State it in one sentence.
**Fails when:** it needs two sentences, or names a style instead of an idea.
**Worked example — fail:** "It's a clean, modern dashboard with a dark theme." (names a style, not an idea)
**Worked example — pass:** "It shows an operator exactly which of their systems needs attention right now, and nothing else." (one sentence, an idea, not an aesthetic)

## 2. PURPOSE
**Question:** Why does every element exist?
**Fails when:** any element has no nameable job.
**Worked example — fail:** A decorative gradient mesh behind a form with no stated reason beyond "it looked flat without it."
**Worked example — pass:** Every card has a stated job (surface a decision, group related data, or hold a single action) and nothing is present without one.

## 3. CLARITY
**Question:** Can someone understand it naturally, unaided?
**Fails when:** it needs instruction, or two things compete for the lead.
**Worked example — fail:** A page with both a hero CTA and an equally weighted secondary link, forcing the user to guess which one the page actually wants.
**Worked example — pass:** One clear primary action per view; everything else recedes until it's needed.

## 4. BALANCE
**Question:** Do all elements belong together?
**Fails when:** any size, gap, or weight follows no system.
**Worked example — fail:** Padding values of 14px, 18px, 22px scattered with no shared scale.
**Worked example — pass:** Every spacing value traces to a stated scale (e.g. 4/8/12/16/24/32/48/64), no exceptions without a named reason.

## 5. HUMANITY
**Question:** Does it respect people?
**Fails when:** inaccessible, impatient, or demanding attention it did not earn.
**Worked example — fail:** A modal that auto-plays sound, can't be dismissed with keyboard alone, and interrupts a task the user didn't ask to interrupt.
**Worked example — pass:** The primary task is completable by a distracted, low-vision, low-bandwidth user without instruction; nothing demands attention without earning it first.

## 6. INTEGRITY
**Question:** Is it honest?
**Fails when:** misleads, imitates a false material, or misrepresents data.
**Worked example — fail:** A bar chart where bar height doesn't match the underlying values, or a countdown timer that resets every time the page reloads (fake urgency).
**Worked example — pass:** Chart proportions match the data exactly; no artificial scarcity or fabricated progress indicators.

## 7. LONGEVITY
**Question:** Will it survive beyond trends?
**Fails when:** the only justification is that it is current.
**Worked example — fail:** "We used this glassmorphism/neubrutalism/[current trend] treatment because that's what's popular right now."
**Worked example — pass:** The stylistic choice is defensible on its own terms — it serves the content, the brand, or the medium — independent of whether it happens to also be fashionable this year.

---

## How to run the gate

Run all seven in order. Do not skip to the ones that are comfortable. State a pass or fail for each — "probably fine" is not a valid answer to any of them. If any check fails, name the specific failure and what would need to change, before presenting the work as finished.

This gate composes with, but does not replace:
- `tool--ux-standards` for a technical WCAG/accessibility conformance audit (Humanity above is the philosophical check, not a substitute for that audit)
- The Visual Quality Checklist in `craft--ui-design-general` and the Cross-Device Quality Gate in `craft--ui-design-cross-devices` (those are execution-completeness checks; this gate is the judgment layer above them)
