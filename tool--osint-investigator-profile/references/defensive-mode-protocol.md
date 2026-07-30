# Counter-OSINT Defensive Mode Protocol

Activates when the operator sets Mode = DEFENSIVE at intake, or says "scan me", "what can someone find on me/us", "what's our OSINT footprint", "what would an adversary find on us", or equivalent.

The same five-phase structure runs, but the geometry inverts: the operator is the subject, and every phase asks what an external investigator would see — not what the operator knows about themselves.

All source labelling, confidence tensors, and the contamination firewall remain in force. The operator's private knowledge of their own situation is `[PRIVATE / quarantined]` and does not enter findings unless it has a verifiable public source.

---

### Defensive Phase 1: Anchor Inventory

The operator provides their own identifiers. The skill confirms what is publicly visible for each.

- Collect: full legal name(s), trading names, aliases, DOB if the operator wishes to surface it
- Collect: all entity names (companies, foundations, vehicles) and all jurisdictions of operation
- Collect: known domains, professional URLs, social profiles, published email addresses
- Collect: names of officers, co-directors, or associates whose exposure the operator wants included
- Confirm which identifiers are publicly indexed vs. only operator-known
- Flag any identifier more exposed than the operator may realise (e.g. personal phone number in WHOIS, DOB in a public filing)

**Output: Anchor Exposure Table** — identifier | expected visibility | actual visibility | delta (more or less exposed than expected)

---

### Defensive Phase 2: Structural Visibility Audit

Map everything publicly retrievable from registries, filings, and document records for the operator's entities and name.

- **Corporate registry**: all current and historic directorships, shareholding positions, registered capital, liquidations — what any researcher can pull for free
- **Filed accounts**: what financial figures are publicly available, year-on-year — revenue trend, net result, equity — visible to any researcher
- **Registered addresses**: is the address a professional domicilio, a residential address, or shared coworking? Each carries different exposure (residential = personal safety risk)
- **Domain WHOIS**: registrant name, email, phone, registrar, creation date — flag any PII visible in WHOIS
- **Court and insolvency indices**: civil and criminal filings indexed under the operator's name or entities
- **PEP and sanctions lists**: confirm operator and named associates are not listed; note proximity to listed persons
- **Published documents**: academic papers, patents, public filings, press releases, parliamentary records — the full written public record

For each record: state access cost (€0 / free / paid) and data freshness.

---

### Defensive Phase 3: Network Visibility and Unintended Associations

Map what an external investigator finds when tracing the operator's connections.

- Identify co-directors, co-shareholders, and co-investors publicly linked to the operator — flag any carrying reputational, regulatory, or legal exposure
- Identify advisors, counsel, or registered-office providers shared with entities that have adverse public records
- Review public deal announcements, press coverage, and event appearances: who is the operator publicly associated with that they may not have intended?
- Check for name-collision risk: other individuals with the same name whose adverse records could be conflated with the operator's in an undifferentiated search
- Flag any association that would activate a nine-agency lens on an external analyst — even if the association is innocent, note that it would draw scrutiny and explain why

**Output: Association Exposure Map** — associate | relationship type | public source | exposure flag (LOW / MEDIUM / HIGH) | mitigation note

---

### Defensive Phase 4: Ghost Inventory (Reverse)

In OFFENSIVE mode, ghosts are things present but incomplete. In DEFENSIVE mode, ghosts are things absent where expected, or inconsistencies that would look suspicious to an external investigator.

Apply the full Ghost Taxonomy in reverse — for each type, ask whether the operator's footprint would trigger that flag:

- **Absence ghost**: missing year of filed accounts? Gap in corporate officer timeline? Domain registered with no indexed content?
- **Address ghost**: registered address not matching operational reality? Would a researcher find nothing at that address?
- **Capital ghost**: publicly announced funding not reflected in registry filings?
- **Timeline ghost**: career or entity gaps a researcher would notice and flag?
- **Document ghost**: references to documents (contracts, filings, press releases) that cannot be located in public sources?
- **Nominee ghost**: entity structure (sole administrator, rapid officer changes, offshore registered office) that would look like a nominee arrangement?

For each ghost: state what an adversary would infer, whether that inference is accurate, and what drives it.

---

### Defensive Phase 5: Adversary Signature

How would a hostile researcher read the operator's footprint? This phase synthesises the full defensive picture into an adversarial interpretation — the story a motivated investigator would build, accurate or not.

- **Journalist narrative**: based only on the public record, what story do the documents tell? Flag if any facts, out of context, support a damaging headline
- **Regulator narrative**: what compliance questions does the public record raise? Which filings, gaps, or associations attract regulatory attention?
- **Litigant's counsel narrative**: in a dispute, which public documents would opposing counsel cite to establish a pattern of behaviour, financial instability, or governance weakness?
- **Competitor narrative**: what public information could a competitor share selectively to damage the operator's reputation with investors, clients, or partners?
- **Worst-case conflation risk**: what could be misread, misattributed, or taken out of context in a way hardest to correct after the fact?

Label every inference `[INFERENCE / based on PUBLIC sources X, Y]`.

---

### Hardening Recommendations

Three concrete actions, sequenced by impact-to-effort ratio. Each rated on three dimensions.

> **Action 1 — Highest impact, lowest effort:** [specific action]
> Impact: [HIGH/MEDIUM/LOW] | Effort: [HIGH/MEDIUM/LOW] | Time to implement: [specific]
>
> **Action 2 — Medium impact, medium effort:** [specific action]
> Impact: [HIGH/MEDIUM/LOW] | Effort: [HIGH/MEDIUM/LOW] | Time to implement: [specific]
>
> **Action 3 — Structural hardening (longer horizon):** [specific action]
> Impact: [HIGH/MEDIUM/LOW] | Effort: [HIGH/MEDIUM/LOW] | Time to implement: [specific]

**Single highest-leverage action**: name the one thing that closes the most exposure with the least operational disruption, and why.

---

