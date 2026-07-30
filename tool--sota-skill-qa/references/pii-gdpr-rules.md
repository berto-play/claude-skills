# PII & GDPR Rules for Claude Code Skills

Run this check before any cloud upload. Zero tolerance — a skill with PII in it is a blocker, not a warning.

---

## Contents

- What Counts as PII
- The Public Figure Exception
- Naming Convention Rules
- Scan Patterns
- What Is NOT PII
- `[who]-[what]` Slug Convention

---

## What Counts as PII

Under GDPR Article 4, personal data is any information relating to an identified or identifiable natural person. For skills, this means:

| Category | Examples | Action |
|---|---|---|
| Real personal names | "Joe Gemayel", "Alberto", a first name that uniquely identifies someone in context | Remove or replace with `[Role]` placeholder |
| Contact data | Email addresses, phone numbers, physical addresses, social handles | Remove |
| Org-internal identifiers | Internal project codenames that identify a person, employee IDs, personal Slack handles | Remove |
| Company names used to identify a person | "<first name> at <client company>" — the combination identifies a private individual | Remove the personal identifier; keep the org name if it is publicly known and relevant |
| Hardcoded stakeholder references | "Run this by Joe before sending", "Anna owns this workflow" | Replace with `[Manager]`, `[Colleague]`, `[Stakeholder]` |

**GDPR Special Categories (Article 9) — never include:**
Health data, biometric data, genetic data, racial or ethnic origin, political opinions, religious beliefs, trade union membership, sexual orientation or gender identity. These categories have heightened protections. No skill should store or process them as examples, test data, or context.

---

## The Public Figure Exception

A real name is permitted only when ALL of the following are true:

1. **The subject holds a verifiable public role.** Examples: published author cited for their book, CEO of a publicly traded company, elected official, public academic with a named institutional affiliation. Not permitted: someone who is well-known internally but not publicly documented.

2. **The reference is to that public role only.** Citing "Daniel Kahneman, *Thinking Fast and Slow* (2011)" is permitted. Citing a private opinion Daniel Kahneman expressed in a meeting is not.

3. **The citation is explicit and traceable.** Name + role + source. "Kahneman (2011)" is traceable. "a leading researcher once said" is not.

4. **The skill would not expose the person to harm.** Even public figures retain rights over private details. Do not include home addresses, personal health information, or anything that could enable harassment — even for public figures.

**When in doubt, remove the name and describe the role instead.**

---

## Naming Convention Rules

Three locations must match exactly. Any mismatch is a D-category structural FAIL.

| Location | Format | Example |
|---|---|---|
| Zip filename (without `.zip`) | `{namespace}--{skill-name}` | `tool--doc-safety-analysis` |
| Internal folder name | `{namespace}--{skill-name}/` | `tool--doc-safety-analysis/` |
| SKILL.md `name:` field | `{namespace}--{skill-name}` | `name: tool--doc-safety-analysis` |

### Valid Namespaces

| Namespace | Use for |
|---|---|
| `tool` | General-purpose analytical, research, or engineering tools |
| `craft` | Design, UX, product, and business craft skills |
| `fun` | Financial analysis and market research |
| `mode` | Session-level behavioral modes and personas |
| `medi` | reserved client-specific content only — not for general-purpose tools |

`medi--` is reserved. If a skill was built for a specific client but contains no client-specific content, it must be renamed to the correct namespace before upload.

### Slug Rules

- **Lowercase only.** `tool--Doc-Safety` → fail. `tool--doc-safety` → pass.
- **Hyphens only.** No underscores (`tool--doc_safety`), no spaces, no camelCase.
- **Double-dash separator.** Single dash (`tool-doc-safety`) → fail.
- **No version numbers in the name.** Version lives in the `metadata:` block, not the filename.
- **Descriptive, not generic.** `tool--helper` → fail. `tool--doc-safety-analysis` → pass.

---

## Scan Patterns

Before upload, scan every file in the skill (SKILL.md + all references/ + all scripts/) for:

```
Real name patterns:   \b[A-Z][a-z]+ [A-Z][a-z]+\b  (filter against known-public list)
Email addresses:      [a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
Known internal names: any first name used as a stakeholder reference in context
Org identifiers:      internal project names, team names not publicly documented
```

For any hit: determine if it is a public figure in their public role (permitted) or a private identifier (must be removed).

Replacement convention:
- Personal name → `[Name]` or a specific role: `[Manager]`, `[You]`, `[Colleague]`, `[Stakeholder]`
- Internal org → `[Company]`, `[Client]`, `[Partner]`
- Internal project → `[Project]`

---

## What Is NOT PII

- Generic placeholder names: `John Doe`, `[Manager]`, `[User]`, `[Client]`
- Public figures cited in their public role with a traceable source
- Organisation names that are publicly known and not used to identify a private individual
- Academic paper authors cited with title + year + publisher
- GitHub usernames on public repositories (these are public by definition)
- Example email addresses clearly marked as fictional: `john@example.com`


---

## `[who]-[what]` Slug Convention

Beyond the structural rules (double-dash, valid namespace, three-location match), the skill slug should read as `[who]-[what]`:

- **`[who]`** — the agent or role the skill embodies. A noun. Who is doing the work?
- **`[what]`** — the primary action it performs. A verb or gerund. What does it do?

| Current name | Passes? | Reason | Proposed alternative |
|---|---|---|---|
| `tool--doc-safety-analysis` | ✓ | Name is well-known and established. Decided: keep as-is. | Keep |
| `tool--benchmarking-research` | ✗ | Two nouns, no agent | `tool--researcher-benchmark` |
| `tool--osint-investigator-profile` | ✓ | Agent (`investigator`) + action (`profile`). Renamed from `tool--osint-investigator-entities`. Decided. | Keep |
| `tool--sota-skill-qa` | ✓ | Evaluator (`evaluator`) + standard (`sota-qa`). Renamed from `tool--skills-creator-audit-qa`. Decided. | Keep |
| `tool--master-librarian` | ✓ | Agent (`librarian`) + implied action (navigate/find) | Keep |
| `tool--critical-thinking` | ✗ | Adjective-noun, no agent | `tool--analyst-reason` |
| `tool--transcript-synthesis` | ✗ | Object-action, inverted | `tool--synthesizer-transcribe` |
| `craft--behavioral-health-designer` | ~ | Has agent but slug is long | `craft--designer-engage` |
| `mode--unhinged` | ✓ | Mode names are single-descriptor — exempt from [who]-[what] | Keep |

### How to apply this gate

1. Split the slug on `-`: `tool--doc-safety-analysis` → `["doc", "safety", "analysis"]`
2. Ask: does the first token read as an agent role? (analyst, researcher, designer, investigator, architect, synthesizer, librarian, builder, coach, navigator…)
3. Ask: does the second token read as an action? (extract, benchmark, profile, audit, reason, synthesize, build, coach, navigate…)
4. If both yes → PASS
5. If no → FAIL. Propose a `[who]-[what]` alternative and present it to the operator. The operator decides whether to rename — this gate does not auto-block, but the proposal is mandatory.

### Exemptions

- **`mode--` skills**: Single-descriptor names (`mode--unhinged`, `mode--direct`) are valid. Modes describe a state, not an agent performing an action.
- **`medi--` skills**: May retain domain-specific slugs when the full name is needed for regulatory traceability.
- **Existing installed skills**: If a skill is already live on `claude.ai`, renaming breaks any cross-skill references. Flag the mismatch, propose the rename, but mark it as deferred until the next version bump.

