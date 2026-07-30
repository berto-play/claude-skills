# Acceptance Criteria Guide

## What acceptance criteria are

The binary, testable conditions that make a user story done. They describe WHAT is
true after the work, never HOW it is built. If every criterion passes, the story is
done; if any fails, it is not. There is no third state.

## Format

Given [starting state], When [action], Then [observable result].

## The 7 rules

1. **Binary.** A stranger can mark pass or fail with no judgment call.
2. **Observable.** The result is visible to the user, or in a file or log the user
   can open. "The function returns X" is not observable; "the output folder
   contains X" is.
3. **One behavior per criterion.** An "and" hiding two behaviors means split it.
4. **Cover happy, sad, and edge.** At least one criterion per story exercises
   failure or an edge condition.
5. **No vague adverbs.** Banned in criteria: fast, easily, properly, gracefully,
   intuitive, seamless, robust. Replace each with a number or an observable.
6. **No implementation detail.** Name results, not libraries, functions, or
   frameworks.
7. **Error criteria name intent and exit,** not exact copy: what the message tells
   the user and what action it offers.

## Good and bad, side by side

(Examples span different products on purpose. The method is domain-agnostic.)

Bad: "The feed should load fast."
Good: "Given a user on a slow connection, When they open the home feed, Then the
first five posts render within 2 seconds and a skeleton placeholder shows until then."

Bad: "Signup works."
Good: "Given a valid email in the signup form, When the user taps Create Account,
Then they land on the onboarding screen and a confirmation email is queued within
30 seconds."

Bad: "Errors are shown gracefully."
Good: "Given a payment is declined, When the user taps Pay, Then the card field
shows 'Card declined, try another card', no charge is created, and focus returns to
the card field."

## Acceptance criteria vs Definition of Done

- **Acceptance criteria:** per story. Different for every story.
- **Definition of Done:** global bar for every story (no crashes, accessibility
  pass, privacy boundary respected, states covered).
Both must pass. Neither replaces the other.

## Why this feeds the build directly

Each criterion becomes one QA test case, one to one. An AI builder treats the
criteria list as its verification checklist before claiming done. Write the
criteria so that "run the list" IS the acceptance test plan, with no translation
step in between.

Under the complex/regulated overlay (`profiles.md`), tag each criterion with the
requirement ID it verifies (for example `FR-012`), so the traceability map closes:
requirement -> acceptance criterion -> test.
