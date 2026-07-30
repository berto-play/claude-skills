# Pipeline Protocol

Detailed procedures for each phase of the skill-builder pipeline. Referenced by
`SKILL.md` when additional specificity is needed. The SKILL.md overview is the
operational reference; this file is the edge-case and recovery guide.

---

## Contents

Phase numbers here match `SKILL.md` exactly. If they ever diverge, `SKILL.md` wins and this
file is the bug.

- Phase 0 detailed: input detection
- Phase 1 detailed: synthesis
- Phase 2 detailed: architecture
- Phase 3 detailed: evaluation cases
- Phase 4 detailed: generation and validation
- Phase 5 detailed: infrastructure
- Phase 6 detailed: candidate and handoff
- Deployment reference: what happens after approval

---

## Phase 0 detailed: input detection

### Detection algorithm

Scan the input for the following in order:

1. **Explicit classification.** Operator says "here is a data dump" or "here is the
   spec" — use that and skip detection.
2. **Structured-spec markers.** Look for labeled fields:
   - Lines starting with "Namespace:", "Name:", "Skill name:", "Behaviors:"
   - YAML-like structure
   - Numbered list of required behaviors with clear labels
   If 3+ labeled fields are present, classify as structured-spec.
3. **Data-dump markers.** Look for:
   - Conversation turn markers (----, dashes, "User:", "Assistant:")
   - Sentences longer than 40 words in prose blocks
   - arXiv IDs (format: YYMM.NNNNN)
   - Multiple pasted document sections separated by horizontal rules
   - Document titles or file names at the top of blocks
   If any 2+ markers are present, classify as data-dump.
4. **Default.** If neither classification triggers, classify as data-dump and note it.

### Classification line

Always output the classification in one line before any analysis:
"Input: data-dump. Proceeding to synthesis." or "Input: structured-spec. Skipping
to Phase 2."

If the classification is uncertain, state it: "Input: ambiguous — treating as
data-dump."

---

## Phase 1 detailed: synthesis

### Extraction order

Run steps in order. Do not merge steps.

**Step 1.1 — Core problem extraction:**
Read all input material. Write one sentence answering: what gap does this skill fill?
What would fail or be missing without it?

If the input contains multiple problem framings (e.g. the operator circled a concept
across multiple threads), choose the most specific one and flag the others as
related-but-out-of-scope.

**Step 1.2 — Namespace identification:**
Apply the namespace decision tree:

```
Does this skill enforce a session-level behavioral pattern
(blocking something, holding through the session)?
→ YES → mode--

Does this skill produce a one-time assessment, routing decision,
or transformation on request?
→ YES → tool--

Does this skill produce a creative or professional artifact
(copy, design, plan, document)?
→ YES → craft--

Does this skill involve a persona, roleplay, or entertainment context?
→ YES → fun--

Is this reserved client-specific content?
→ YES → medi--
```

If two namespaces seem to fit, choose the primary function. Note the secondary fit.
Example: "Primary: mode-- (session-level enforcement). Secondary fit: tool-- (includes
an assessment step) — but the session-hold behavior makes mode-- the right namespace."

**Step 1.3 — Behavior extraction:**
Extract behaviors as a two-column list:

| Must do | Must not do |
|---|---|
| [behavior] | [behavior] |

Pull from all input material. For mode-- skills, the "must not do" column is as
important as the "must do" — it defines the calibration clause.

**Step 1.4 — Variant space extraction:**
Identify distinct cases the skill must handle. For mode-- skills: what are the
attack/manipulation vectors? For tool-- skills: what are the boundary conditions and
failure modes? For craft-- skills: what are the quality edge cases?

Name each variant. If the input already names them (e.g. "Family D, Family I"),
preserve those names.

**Step 1.5 — Research grounding extraction:**
List every citation, benchmark, study, or prior work mentioned in the input. For each:
- Full reference (author, title, venue/journal, year, arXiv ID if applicable)
- Verification status: verified (checked this session) or unconfirmed (from input,
  not independently checked)
- What claim it supports

Flag as UNCONFIRMED any citation from a preprint (arXiv) that has not been
independently verified. Do not cite externally until checked.

**Step 1.6 — Spec production:**
Produce the structured spec and present it to the operator. Do not proceed without
confirmation. The spec is the contract for Phase 2; mistakes here propagate to all
downstream phases.

### Synthesis failure modes

| Failure | Signal | Recovery |
|---|---|---|
| Input is too thin to synthesize | Under 100 words, no behaviors extractable | Ask the operator for a richer description or sample output |
| Contradictory behaviors in input | Two threads disagree on a core behavior | Flag both positions, ask operator to arbitrate |
| Namespace is ambiguous after the decision tree | Fits two namespaces equally | Build as the more restrictive namespace (mode-- > tool-- > craft--> fun--) and note the decision |
| No research grounding | Input contains no citations | Proceed without citations; note in the spec that the grounding section is empty |

---

## Phase 2 detailed: architecture

### Overlap check procedure

```bash
# From the Claudio AI workspace root
grep -r "<candidate-skill-name>" "_Claude Skills Repo/live/" --include="*.md" -l
grep -r "<synonyms>" "_Claude Skills Repo/live/" --include="*.md" -l
```

If hits are found, read the relevant SKILL.md and assess whether the scope overlaps.
Report to the operator before proceeding. Options:
1. The existing skill covers the gap — stop; recommend updating the existing skill
   instead of creating a new one.
2. The new skill is adjacent but distinct — proceed; note the relationship in the
   Boundaries section of the new SKILL.md.
3. The existing skill should be retired in favor of the new one — flag to operator;
   do not retire automatically.

### Reference file planning

For each planned reference file, produce:
- Filename (lowercase, hyphens, no spaces)
- What it covers (one sentence)
- Which section(s) of SKILL.md will reference it
- Approximate structure (sections it will contain)

Confirm this plan with the operator or proceed if the scope is unambiguous.

---

## Phase 3 detailed: evaluation cases

Cases are written **before** the documentation. The order is the point: writing docs first
means documenting imagined problems.

### Baseline procedure

1. Run the target task in a clean context with **no** skill loaded.
2. Record verbatim what actually failed. Not what you expect to fail.
3. Write one case per recorded failure.

A case with no observed baseline failure behind it is speculation. Mark it as such or cut it.

### Coverage floor

Minimum three cases. Realistically: one per trigger list (positive, negative, ambiguous)
plus every safety category in [contracts.md](contracts.md). For `untrusted-input` skills the
five `SECURITY-STANDARD.md` §6 injection categories are additional to this, not part of it.

### Threshold discipline

Write acceptance thresholds into `cases.json` before the first run. Changing one after
seeing results invalidates that run — say so, change it, and re-run everything. Do not
retro-fit a number to a result you already hold. Full rule in
[contracts.md](contracts.md).

### Evaluation-case failure modes

| Failure | Signal | Recovery |
|---|---|---|
| No baseline run performed | Cases describe hypotheticals, no recorded failure text | Stop; run the baseline before writing cases |
| Threshold set after results | A number that exactly clears the observed score | Void the run, reset the threshold, re-run |
| Safety category missing | A contracts.md category has no case | Hard blocker; add it before Phase 4 |

---

## Phase 4 detailed: generation and validation

### Description length check

The frontmatter description must be 1000 characters or under for capability skills.
This is QA gate A2. Check with:

```bash
echo -n "[description text]" | wc -c
```

If over 1000: trim by removing inline examples, cutting redundant phrases, or moving
content into the SKILL.md body. The description should state: what it does, how it
does it (key mechanism), and what it does NOT do. That is enough.

### Internal validation procedure

The authoritative check is the script, not a hand-run list:

```bash
python3 scripts/validate_skill.py <skill-dir>     # must exit 0
```

It enforces the six structure rules in `SKILL.md` Phase 4: body under 500 lines, references
exactly one level deep, a table of contents on any reference file over 100 lines, forward
slashes throughout, a third-person description stating what and when, and frontmatter keys
top-level rather than nested under `metadata:`.

For each blocker the script reports:
1. State which rule failed and why
2. State the specific fix
3. Apply it
4. Re-run the script

A blocker is not a warning. Do not proceed to Phase 5 until the script exits 0, or the
operator has explicitly acknowledged a known-fail with a remediation plan. The script checks
form, not quality — exit 0 does not mean the skill is any good.

### Reference file completeness check

For each reference file the SKILL.md references, verify:
- The file exists at the stated path (`references/<filename>.md`)
- The filename in SKILL.md matches the actual filename exactly (case-sensitive)
- The file's content covers what the SKILL.md says it covers

A referenced file that does not exist is a hard blocker. A referenced file whose
content does not match the stated scope is a soft flag — note it but do not block.

### Research citation treatment

Every citation in any generated file must be labeled. Labels:
- `(confirmed)` — fetched or searched this session; content verified
- `(unconfirmed)` — from input material; not independently checked; include the flag
  "Do not cite externally until checked."
- `(training data)` — from prior knowledge, not retrieved this session

If a citation is central to a key claim in the skill (e.g. the mechanism behind a
protocol), mark it as UNCONFIRMED in the source register and add a note that the
claim should be reverified before the skill is used in high-stakes contexts.

---

## Phase 5 detailed: infrastructure

### Folder creation

```bash
# Capability skill
mkdir -p "_Claude Skills Repo/live/capability/<name>/references"

# Operational skill
mkdir -p "_Claude Skills Repo/live/operations/<name>/references"
```

Verify the folder exists before writing files. If the folder already exists (unlikely
but possible if a previous partial run happened), check for conflicting files before
overwriting.

> **DEPLOYMENT STAGE ONLY.** SKILL.md forbids the builder from running symlink,
> pack or upload steps. Everything from here to the end of this section happens AFTER
> QA passes and the operator approves — it is documented here so the deploying
> party has the exact commands, not so the builder runs them.

### Symlink creation (capability only)

```bash
ln -s "$(pwd)/live/capability/<name>" ~/.claude/skills/<name>
```

Run from inside `_Claude Skills Repo/`. The absolute path avoids relative-path
resolution issues.

Verify:
```bash
ls -la ~/.claude/skills/<name>
# Must show: lrwxr-xr-x ... -> <skills-repo>/live/capability/<name>
cat ~/.claude/skills/<name>/SKILL.md | head -3
# Must show the frontmatter of the new skill
```

### Packaging (capability only)

```bash
cd "_Claude Skills Repo" && skills pack <name>
```

Post-pack verification:
```bash
unzip -l "packaged/<name>.zip"
# Must show:
# <name>/SKILL.md
# <name>/references/<file1>.md
# <name>/references/<file2>.md
# (etc.)
```

If the zip is missing any expected file, re-run `skills pack`. If still missing,
check the file exists in `live/capability/<name>/` and has write permissions.

### Changelog entry format

```markdown
## YYYY-MM-DD

**Session:** [Brief context — operator request, what triggered this build.]

### Created
- `live/[capability|operations]/<name>/SKILL.md` — [one-paragraph description of
  what the skill does, key sections, key technical decisions. Enough to reverse the
  action from reading the changelog alone.]
- `live/.../references/<file1>.md` — [one sentence: what it covers.]
- `live/.../references/<file2>.md` — [one sentence: what it covers.]
- Symlink `~/.claude/skills/<name>` → `live/capability/<name>` (verified: resolves,
  SKILL.md reachable through link). [Omit for operational skills.]

### Packaged
- `packaged/<name>.zip` — [first zip / rebuild]. Contents: [list files]. Verified clean.
  [Omit for operational skills.]

### Known Pending
- [Any remaining steps — cloud upload, Notion, GitHub, QA, calibration.]
```

Write this entry at the TOP of the changelog (after the `---` separator following
the file header), not at the bottom.

### SYMLINKS.md update (new capability symlinks only)

Location: `Claudio AI/xEngine/governance/SYMLINKS.md`

1. Add a row to the "Capability skills" table, alphabetically by skill name:
   ```
   | `~/.claude/skills/<name>` | `_Claude Skills Repo/live/capability/<name>` | dir symlink | Claude Code runtime |
   ```
2. Increment the symlink count in the "Active symlinks" section header.
3. Bump the "Last reviewed" date at the bottom of the file.

Do not update SYMLINKS.md for operational skills (they have no symlinks).

### Recovery from partial Phase 5

If Phase 5 is interrupted:
1. Read the changelog to see what was completed before the interruption.
2. Check which steps were verified (not just attempted).
3. Resume from the first unverified step.
4. Do not re-run completed verified steps (re-creating a symlink that already exists
   will error; re-running pack is safe but may produce a duplicate log line).

---

## Phase 6 detailed: candidate and handoff

### Emit the candidate

Write `candidate.json` per Contract A in [contracts.md](contracts.md) to
`playground/<name>/.candidate/<version>/`. Generate `source_checksum` and
`checksum_manifest` from the script — never by hand:

```bash
python3 scripts/skill_checksum.py <skill-dir> --json
```

`self_certified` stays `false`. On a partial build emit **no** candidate.json at all and
report `INCOMPLETE`, naming the missing file. A half-written candidate that looks complete
is the worse outcome.

### Handoff report

State: path, version, checksum, what was built, known limitations, next action, and that it
is **unverified**. No QA report exists yet, so nothing about quality is known. "Known
limitations: none" is almost always false and reads as carelessness.

Then stop. Grading is `tool--sota-skill-qa`; deployment needs the operator after that.

---

## Deployment reference: what happens after approval

Everything below is **out of scope for the builder**. It is documented so the deploying
party has exact commands, not so this skill runs them.

### Pre-ship checklist

Run before presenting the status table to the operator:

- [ ] All Phase 5 steps verified (not just run)
- [ ] Full QA (`tool--sota-skill-qa`) run or explicitly deferred with
  operator acknowledgment
- [ ] Changelog entries exist in BOTH changelogs
- [ ] SYMLINKS.md updated if a new symlink was created
- [ ] No known broken references in SKILL.md (all referenced files exist)

### Ship command sequence

If operator says go:

```bash
# 1. Cloud upload
skills ship <name>
# OR if ship is unavailable:
# skills cloud-sync (uploads all changed skills)

# 2. Verify cloud upload
skills cloud-status
# Should show <name> as in-sync

# 3. Notion Skills Inventory update
# Invoke tool--system-maintenance Step 5 or update manually
# Page: 36c15c23-aec3-81a5-bdf2-c2f06ac4269b

# 4. GitHub push
skills push
# OR: git -C "_Claude Skills Repo" push origin main --tags
```

### Completion Report format

After all ship steps complete, report:

```
Completion Report — <name>

| Step | Status | Verified |
|---|---|---|
| Files written | ✅ | SKILL.md + N reference files |
| Internal QA | ✅ / ⚠️ | 9/9 or N/9 with noted gaps |
| Symlink | ✅ / N/A | resolves / operational |
| Packaged | ✅ / N/A | N files, clean / operational |
| Changelogs | ✅ | both updated |
| SYMLINKS.md | ✅ / N/A | updated / no new symlink |
| Full QA (20/20) | ✅ / ⚠️ | score |
| Cloud upload | ✅ / ⏳ | verified / pending |
| Notion Inventory | ✅ / ⏳ | verified / pending |
| GitHub push | ✅ / ⏳ | verified / pending |

Bottom line: [Done / Partially done — N steps pending / Blocked on X]
```

A step is ✅ only after verification. ⏳ means pending operator go or tool availability.
⚠️ means completed with a known issue that was acknowledged.
