# Ghost Taxonomy

A "ghost" is any element of the target's footprint that is present but structurally
incomplete, absent where expected, or inconsistent with the surrounding picture.
Ghosts are not accusations. They are open questions that warrant follow-up.

Each type has: definition, diagnostic questions, and default next action.

---

## 1. Financial Ghost

**Definition:** Entity exists, is active, but has filed no accounts or consistently
files accounts showing zero revenue with no plausible explanation (holding company,
newly incorporated).

**Diagnostic questions:**
- How many fiscal years since incorporation?
- Are any accounts filed? If yes, what do they show?
- Is the entity labelled a holding company in its ATECO code?

**Default next action:** Pull visura ordinaria + any bilancio depositi from the
Registro Imprese. Determine whether accounts were ever filed. If accounts exist but
show zero revenue for >3 years with no dormancy notice, flag as unexplained.

---

## 2. Identity Ghost

**Definition:** An officer is named in a registry record but has no verifiable DOB,
fiscal code, address, or any other identifying anchor. Could be a nominee director,
a transcription error, or a real person deliberately obscured.

**Diagnostic questions:**
- Does the name appear in any other registry filing with fuller details?
- Is this the sole officer on multiple unrelated entities (nominee pattern)?
- Does the name match any public professional profile?

**Default next action:** Search the name on Registro Imprese across all entities.
If the name appears as sole administrator on >3 unrelated SRLs with no other public
footprint, flag as probable nominee director.

---

## 3. Infrastructure Ghost

**Definition:** Domain registered, social profiles created, or office address listed,
but no live services, no actual web presence, and no evidence of operational activity.

**Diagnostic questions:**
- When was the domain registered relative to incorporation?
- What does the WHOIS record show for registrant and registrar?
- Does a web snapshot (Wayback Machine) show historical content?

**Default next action:** Check Wayback Machine for historical content at the domain.
Check LinkedIn for employee count. An entity with 0 employees on LinkedIn and a
domain registered within 30 days of a press announcement is a strong ghost signal.

---

## 4. Litigation Ghost

**Definition:** Entity or individual is named in press as involved in litigation,
dispute, or regulatory action, but no corresponding court or regulatory record is
findable in public indices.

**Diagnostic questions:**
- Was the press coverage specific (court name, case number) or vague?
- Could the litigation be under a different entity name?
- Is the jurisdiction one where proceedings are not publicly indexed?

**Default next action:** Check Portale dei Servizi Telematici (Italy) or equivalent
civil registry. If the press coverage named a specific court and year, search that
combination. Absence from public index may simply reflect sealed proceedings.

---

## 5. Beneficial Ownership Ghost

**Definition:** PSC register, UBO filing, or equivalent beneficial ownership record
is blank, shows "no PSC / no UBO", or names a corporate entity (not a natural person)
as the ultimate beneficial owner with no further chain resolved.

**Diagnostic questions:**
- Is the corporate UBO itself registered somewhere with a resolved beneficial owner?
- Does the jurisdiction have a mandatory UBO register with enforcement?
- Is this a type of entity exempt from UBO disclosure?

**Default next action:** Trace the corporate UBO entity. Pull its registry record.
If the chain terminates in a BVI, Cayman, or trust structure with no public disclosure,
flag as intentionally opaque. Note that opacity is not proof of wrongdoing but is a
material due-diligence flag.

---

## 6. Press Ghost

**Definition:** Entity or individual is commercially active (has clients, employees,
revenues) but has zero media coverage, no press mentions, and no industry presence
beyond its own channels.

**Diagnostic questions:**
- What is the entity's revenue / employee count?
- Is the absence consistent with the sector (B2B infrastructure vs consumer)?
- Could the entity operate under a different commercial name?

**Default next action:** For an entity above €1M revenue or 20 employees, zero press
coverage is statistically unusual. Search the sector trade press, LinkedIn job postings
history, and any procurement databases. Press ghost may indicate a deliberate low
profile, a very early stage, or a shell.

---

## 7. Connection Ghost

**Definition:** Entity or individual appears in multiple adjacent target networks
(co-investors, co-directors, shared counsel) but is never directly linked — always
one degree of separation from the main subject.

**Diagnostic questions:**
- How many target networks does this actor appear adjacent to?
- Do the connections share timing (all 2022-2024)?
- Is this actor also a ghost on any other taxonomy dimension?

**Default next action:** Maintain a cross-target ledger for this actor. If they
appear in ≥3 adjacent networks without ever being a named direct party, consider
them a potential hidden common-control signal. Pull their full registry record.

---

## 8. Career Ghost

**Definition:** A gap of ≥18 months in a person's publicly documented career history
with no explanation. May indicate a period of dormancy, a failed venture scrubbed
from the record, a legal restriction, or deliberate omission.

**Diagnostic questions:**
- What was the person doing in the period immediately before and after the gap?
- Does any registry record show an entity active during the gap period?
- Does the gap coincide with any known market event, litigation, or regulatory action?

**Default next action:** Search Registro Imprese and LinkedIn for any entities the
person may have been associated with during the gap. Cross-reference with court filing
indices for the same period. A gap coinciding with a known failed venture that has
been scrubbed from self-reported profiles is a low-grade flag; a gap coinciding with
criminal proceedings is high-grade.

---

## 9. Capital Ghost

**Definition:** A funding round, investment, or capital raise has been announced
(press, LinkedIn, company blog) but no corresponding change is visible in official
registry records (filed cap table, share allotment, increased registered capital).

**Diagnostic questions:**
- What did the announcement claim (amount, investors)?
- What does the current registry filing show for registered capital?
- Is the timing consistent with filing deadlines in the jurisdiction?

**Default next action:** Pull the visura ordinaria to check filed capital and any
changes to the quote sociali. In Italy, an SRL cap table change requires a notarised
act filed with the Registro Imprese within 30 days. If the announcement is >6 months
old and no change has been filed, the round may not have closed as announced.

---

## 10. Address Ghost

**Definition:** The entity's registered address is a domicilio fiscale provider,
virtual office, law firm, or accountant's address — not a real operating location.

**Diagnostic questions:**
- How many other entities share this registered address?
- Is the address a known virtual-office or registered-agent provider?
- Is there any evidence of actual operations at the address?

**Default next action:** Search the address on Registro Imprese to count co-registrants.
>10 entities at one address indicates a virtual office or domicilio fiscale. This
is not inherently suspicious (common for early-stage startups) but means the address
provides no operational intelligence.

---

## 11. Continuity Ghost

**Definition:** Entity was dormant or near-dormant (zero revenue, zero employees,
minimal filings) for ≥3 years, then suddenly became active — new officers, new
capital, new transactions — without a clear narrative for the reactivation.

**Diagnostic questions:**
- What changed in the filing record between the dormant period and the active period?
- Are the current officers the same as the original officers?
- Does the timing of reactivation coincide with any external event?

**Default next action:** Compare the bilancio filings across the full corporate life.
Identify the year the entity became active. Identify who the officers are in each
period. Shell reuse for a new purpose is a common Italian corporate structure; it is
not inherently problematic but warrants explanation.

---

## 12. Translation Ghost

**Definition:** The target has substantially different footprints in different
languages. English-language presence is notably thinner than Italian-language (or
vice versa), suggesting the target manages its profile differently for different
audiences.

**Diagnostic questions:**
- What does a native-language (Italian, Arabic, French, etc.) search return vs.
  English?
- Is the discrepancy consistent with the target's stated market focus?
- Does the native-language record include claims, controversies, or affiliations
  absent from the English record?

**Default next action:** Run all key searches in the target's primary operating
language before concluding the search phase. For Italian targets, this means separate
Google.it searches, ATECO-specific trade press, and Italian-language LinkedIn. A
target that presents differently in Italian vs. English is managing its international
reputation separately from its domestic one.
