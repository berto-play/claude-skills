# Behavioral Matrix Method

The behavioral matrix (section 7) is the app's behavioral truth table. It has three
bands, and every pack fills all three:

1. **Normal:** each meaningful user action to its expected result. Sweep the UI: every
   button, field, gesture, and menu item. If a control exists, its normal behavior is a
   row.
2. **State transitions:** each move between states to what the user sees. Sweep the
   states from section 6 and connect them: empty to populated, signed-out to signed-in,
   loading to loaded, running to done, offline to synced. Each transition is a row.
3. **Failure and recovery:** what breaks and how it recovers. The rest of this file is
   the method for finding this band exhaustively.

## Definitions (failure band)

- **Edge case:** one variable at its limit (empty list, huge file, no permission).
- **Corner case:** two or more limits at once. Harder to reproduce, usually worse.
- **Sad path:** a valid user action that works against the goal (cancel mid-task,
  wrong target, double tap).

Heuristics and UX laws cover the happy path; bands 1 and 2 above cover normal behavior
and transitions; this method covers everything that can go wrong.

## The 8 hunting grounds

Visit all 8 for every spec. A ground that does not apply stays listed, marked N/A
with a reason. The trigger questions are domain-neutral; the examples span UI input,
API payloads, files, and CLI args. `profiles.md` adds a sharper edge lens per profile.

| # | Ground | Trigger questions |
|---|--------|-------------------|
| 1 | Input | Empty? Missing required field? Wrong type? Out of range? Huge? Malformed or truncated? Duplicate? Unexpected characters or encoding? Payload claims one type but is another? |
| 2 | Volume and scale | 0 items? 1 item? 10,000 items? One giant item? A response or output larger than memory, disk, or a page limit? |
| 3 | State and interruption | Quit, sleep, or crash mid-task? Power or connection loss? Second run or request while one is in flight? Re-run over existing output? Session or token expires mid-flow? |
| 4 | Permissions and environment | No read or write access? OS or user permission prompt denied? Missing or wrong-version dependency? Unconfigured first run vs configured later runs? |
| 5 | Storage and network | Storage full? A drive, service, or endpoint disconnects? A path or resource renamed or moved mid-task? Slow or partial response? A cloud file not yet downloaded locally? |
| 6 | Concurrency | Two actions or callers at once? The same record edited by two users? Underlying data changes during processing? Last-write-wins vs conflict? |
| 7 | Time | Long-running task? Timeout and retry policy? Work crossing midnight when names or windows carry dates? Clock or timezone change? Stale cache? |
| 8 | Human factors | Wrong target selected? Cancel then instant retry? Button or call double-fired? Wrong input supplied? Expects last settings remembered? Misreads progress or status? |

## Corner-case generator

Pick two grounds and cross them. Examples: huge input x disconnecting drive;
permission prompt x batch run; sleep x 10,000 files; cancel x half-written output.
Add the crossings that threaten data or trust.

## Writing the rows

Each found case becomes one matrix row: Case, Trigger, Expected behavior, Severity.

Severity triage:
- **v1 must-handle:** breaks the core job or risks data. Data loss is always here.
- **Degrade gracefully:** skip the item, report it in the end-of-run summary.
- **Document only:** rare and low-harm; noted so the builder decides consciously.

## Rules

1. The builder never guesses. If the expected behavior is unknown, the case goes to
   Open Questions, not to a guess.
2. No silent skips. Every skipped or failed item appears in the user-visible summary.
3. Source data is sacred. Any case that could modify or lose input or source-of-truth
   data (a file, a record, a user's saved work) is v1 must-handle, and the expected
   behavior must preserve the source.
4. Prefer prevention over messaging: if a case can be made impossible (disable the
   run button while running), that beats a well-worded error.
