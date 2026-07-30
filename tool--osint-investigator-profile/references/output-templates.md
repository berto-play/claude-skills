# Output Templates — OSINT Investigator

## OFFENSIVE Report Template

```
## OSINT REPORT: [TARGET NAME]
Date: [date]  |  Mode: OFFENSIVE  |  Operator: [if provided]

### IDENTITY ANCHORS
[confirmed identifiers, source-tagged]

### STRUCTURAL MAP
[corporate, regulatory, litigation footprint — all entries source-tagged]

### NETWORK MAP
[relationships, cross-target flags if applicable]

### GHOST INVENTORY
[ghost type | evidence gap | next action]

### BEHAVIOURAL SIGNATURE
[five elements, all INFERENCE-labelled]

### NINE-AGENCY LENSES
[only activated lenses]

### PEP CONCENTRIC MAP
[if applicable]

### PAYWALL LEDGER
[see Paywall Ledger section below]

### OPERATIONAL TRIAGE
[see Operational Triage section below]
```

---

## DEFENSIVE Report Template

```
## OSINT DEFENSIVE SCAN: [OPERATOR NAME / ENTITY]
Date: [date]  |  Mode: DEFENSIVE  |  Scope: [names and entities in scope]

### ANCHOR EXPOSURE TABLE
[identifier | expected visibility | actual visibility | delta]

### STRUCTURAL VISIBILITY AUDIT
[registry, filing, domain, document exposure — all entries source-tagged with access cost]

### ASSOCIATION EXPOSURE MAP
[associate | relationship type | public source | exposure flag | mitigation note]

### GHOST INVENTORY (REVERSE)
[ghost type | adversary inference | evidence | accuracy assessment]

### ADVERSARY SIGNATURE
[journalist narrative | regulator narrative | litigant narrative | competitor narrative | worst-case conflation risk]

### HARDENING RECOMMENDATIONS
[Action 1 | Action 2 | Action 3 — each with impact / effort / time to implement]
[Single highest-leverage action]

### PAYWALL LEDGER
[paid sources that would expand or clarify the operator's own public record]

### OPERATIONAL TRIAGE
[€0 / €100 / €1,000 tiers — costs here are hardening costs, not research costs]
```

---

## Paywall Ledger Format

List every paid source that would materially advance the investigation. For each:
- Source name and URL
- What it contains (specific to this target)
- Estimated cost
- Unlock value (HIGH / MEDIUM / LOW)
- Cheaper or free alternative if one exists

End with: **Single most valuable document** — name the one paid record that closes the most open questions, at what cost, and why.

Read `references/paywall-atlas.md` for the source index by jurisdiction and target type.

---

## Operational Triage Format

Three explicit decision-grade outputs. No laundry lists.

> **If you have €0 and 1 hour:** [one specific action]
>
> **If you have €100 and 1 day:** [one specific sequence, with costs]
>
> **If you have €1,000 and 1 week:** [one specific sequence, with costs and time allocation]
