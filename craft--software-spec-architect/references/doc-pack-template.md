# The Document Pack Template

Fixed order. The chosen **product profile** (`profiles.md`) decides which sections are
filled in full and which are marked N/A with one line saying why, so the reader knows a
section was considered, not forgotten. A section is never silently dropped, and never
force-filled with content the profile does not need.

**Pack header:** title (working name is fine), version (v0.1 first delivery), date,
status (Draft for review / Approved).

---

## 1. One-line vision

**Purpose:** the whole product in one sentence.
**Rule:** shape: "A [platform] app that [job] for [user], [key constraint]." No
adjectives that do not constrain the build.
**Examples (illustration only, any domain):** "An iOS app that lets runners share
routes with friends on a live map." / "A web dashboard that shows a small team its
subscription revenue at a glance."

## 2. Problem

**Purpose:** why this deserves to exist.
**Rules:** 3 to 6 lines. The pain, who has it, the current workaround, why the
workaround fails. No market-size prose.

## 3. Users

**Purpose:** who the builder is serving.
**Rules:** primary persona first: their technical level and context. Name the real
person when it is personal software. Add a secondary persona only if it changes a
build decision.

## 4. Lifecycle workflow

**Purpose:** the happy path across the full life of the product, not just one session.
The profile decides the shape.
**UI profiles (app, web, mobile):** three passes, each numbered steps of one user action
or one system response.
- **First run (fresh device):** download or install, first launch, any permission or
  setup prompt, onboarding, the moment the user reaches first value. 6 to 12 steps.
- **Returning use (has data and settings):** what a user who already onboarded sees and
  does on a normal later launch. What is remembered, what is skipped, what resumes.
- **Empty vs populated:** if the returning user can arrive with no data yet (account but
  nothing created), note how that differs from the fully populated case.
Write first run first. If there is no install step (pure web), say so and start at first
visit.
**Non-UI profiles (API, CLI, pipeline, service):** replace the above with **first-use /
init** (how it is configured, authenticated, or deployed the first time, reaching first
successful call or run) and **steady-state operation** (a normal run or request cycle,
what persists between runs). Same intent: the full life, not one moment.

## 5. User stories + acceptance criteria

**Purpose:** the testable contract for the build.
**Rules:**
- Story shape: "As [user], I want [capability], so that [outcome]."
- Under each story: 2 to 5 acceptance criteria in Given/When/Then. Full rules in
  `acceptance-criteria-guide.md`.
- Coverage: every step of section 4 is owned by at least one story, plus the sad
  paths that block the job.
- A story with no acceptance criteria is a wish, not a story.

## 6. Screen states

**Purpose:** no screen is ever undefined.
**Rules:** one table per screen or flow. Rows are states: loading, empty (first-time,
no data yet), populated (returning user with data), success, partial, error, permission
denied, and offline where a network exists. Columns: what the user sees, what they can
do next. Note responsive behavior (phone, tablet, desktop) where the platform spans them.
**Hard rule:** the system never lies about its state. Progress shown must reflect
true progress.

## 7. Behavioral matrix

**Purpose:** the app's behavioral truth table, so the builder is never guessing what
should happen in any situation, normal or broken. This is the highest-leverage section
for an AI builder.
**Rules:** table with columns: Situation, Trigger, Expected behavior, Severity. Cover
three bands, not just failures:
- **Normal:** each meaningful user action to its expected result (tap, submit, select,
  navigate).
- **State transitions:** each move between states to what the user sees (empty to
  populated, signed-out to signed-in, running to done, sync start to sync complete).
- **Failure and recovery:** the edge and corner cases from the 8 hunting grounds in
  `edge-case-method.md` (bad input, offline, timeout, permission denied, conflict,
  interruption, concurrency), each to its recovery.
Severity: "v1 must-handle" (breaks the core job or risks data), "degrade gracefully"
(handle and report), "document only" (rare, noted). Data-loss situations are always v1
must-handle. Visit all 8 hunting grounds or mark one N/A with a reason.

## 8. Errors and recovery

**Purpose:** failure is designed, not improvised.
**Rules:** for each error class, state: the message intent in plain words (name the
problem and the exit, never a code alone), what the user can do (retry, skip, change
target, open log, reveal output), and what is never done (silent failure, data loss,
blaming the user). Every skipped item must appear in the end-of-run summary. No
silent skips.

## 9. Non-functional requirements

**Purpose:** the qualities that make it feel professional.
**Rules:** every NFR needs a **measurable target**, written as a quality scenario:
*stimulus + operating condition + expected response + how it is measured*. No vague
adverbs (fast, secure, seamless). Cover at least four:
- Performance: what must feel instant (name a number); what may take time but must show
  true progress.
- Accessibility: keyboard-only path, screen reader labels, contrast, text size. For UI
  profiles, name a **WCAG 2.2 target level (A or AA)**, not "screen-reader support."
- Reliability: source data never modified; crash and resume behavior; atomic writes.
- Compatibility: OS versions, offline behavior.
The cloud/distributed and complex profiles add more categories (availability, recovery
with RTO/RPO, scalability, observability). Those live in `profiles.md`; pull them in only
when the profile calls for them.

## 10. Privacy, security, and data boundary

**Purpose:** where data lives and who may touch it. This section is never cut.
**Rules (all profiles):** state what data is touched, where it lives, and the **boundary
derived from the real architecture**, not a fixed default: a local tool keeps data on the
device; a cloud app names what is sent, to which service, and why. State telemetry
(default: none unless declared), what logs contain (metadata only: counts, statuses,
paths; never content values), and retention. For health or financial data the boundary is
a hard constraint, and any low-confidence or sensitive result is flagged for human
verification, never silently trusted.
**Security sub-block (fires for cloud, multi-tenant, auth, or sensitive-data profiles):**
authentication and authorization, an access-control matrix (who can do what), tenant or
client isolation, secrets management, trust boundaries, abuse cases, rate limiting, and
audit events. For applicable web requirements reference OWASP ASVS and NIST SSDF via
`profiles.md`. A single-user local tool writes "No auth, single local user" and stops.

## 11. Data model (conditional)

**Fires when:** the software persists anything (files, records, settings, accounts).
If it stores nothing, write "No persistence in v1" and stop.
**Purpose:** the shape of what is stored, so state is unambiguous. This is shape, not
schema: no SQL, no migrations.
**Rules:** list each entity, its key fields, and its relationships (one-to-many, etc.),
in a small table or list. Note where each lives: on device, in a keychain, in the cloud.
Name what is portable across a reinstall or a new device, and what is local-only.

## 12. System shape + components (conditional)

**Fires when:** the software has more than one part (a backend, external services, a
sync layer, background workers). A single self-contained client can write "Single local
client, no services" and stop.
**Purpose:** the parts and their boundaries, so the builder knows the seams before
writing code.
**Rules:** name each part, its one-line responsibility, and who it talks to. A short
list or a simple diagram-in-words is enough; for a real system use C4 **context and
container** level only (skip component diagrams unless one earns its place). Note external
services and what happens when each is unavailable. No framework religion, just the shape.
**Contract sub-rule (API, event, or integration profiles):** name the machine-readable
contract each interface will have and where it lives: HTTP APIs → OpenAPI, event/message
interfaces → AsyncAPI, structured payloads → JSON Schema. Give the shape (endpoints or
events, request/response, auth, errors, retry/timeout/conflict behavior), not the full
document. Standards and links are in `profiles.md`.

## 13. Technical decisions (conditional)

**Fires when:** a non-obvious technical choice was made (framework, storage, sync,
offline strategy, auth). If every choice is an obvious default, write "No load-bearing
tech decisions yet; see Open Questions" and stop.
**Purpose:** record the few choices a builder must not silently reverse (ADR-lite).
**Rules:** one row per decision: Decision, Why, Alternatives rejected, Consequence.
Keep to the 2 to 5 that actually shape the build. Skip obvious defaults.

## 14. Non-goals

**Purpose:** protect v1 from drift.
**Rules:** list all confirmed exclusions, **no minimum count** and none invented. One line
each: what v1 will not do, and, only if useful, where that job lives instead. If the
operator has named no exclusions, ask for them or write "None stated yet" rather than
manufacturing exclusions to fill the section.

## 15. Success metrics

**Purpose:** a finish line the operator can see without instrumentation.
**Rules:** 2 to 4 metrics, each observable by the operator. Include one sentence in
the shape: "v1 is done when [single test]."

## 16. Open questions + decisions needed

**Purpose:** honesty about what is unsettled.
**Rules:** two lists.
- Decisions needed: choices only the operator can make. Each gets a recommendation
  and a one-line rationale.
- Open questions: unknowns to resolve during the build.
- Every "Assumed:" marker from the pack is repeated here.

## 17. Build + install notes

**Purpose:** the handoff to whoever builds it, including how it ships to a real device.
**Rules:**
- Platform and framework suggestion; existing assets to reuse (name real paths and
  tools only if they exist).
- Suggested build order, walking skeleton first.
- A feasibility read: what is easy, what is risky, and the single hardest part.
- Packaging and install/deploy, per profile: a UI product names how it installs on a
  fresh device (App Store, signed .app or .dmg, PWA, web URL) and what user state is
  portable versus recreated; a service/API/pipeline names how it deploys and is
  configured in a new environment. State what a clean first launch or first run requires.
- One handoff line naming which skill, agent, or person builds it.
