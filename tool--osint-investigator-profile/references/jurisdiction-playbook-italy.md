# Jurisdiction Playbook: Italy

Primary sources, identifier formats, search instructions, and access notes
for Italian OSINT investigations.

---

## Key Identifiers

| Identifier | Format | Use |
|---|---|---|
| Codice Fiscale (CF) | 16-char alphanumeric for persons; 11-digit for entities | Primary tax ID; appears on all official documents |
| Partita IVA | 11-digit numeric | VAT number for entities and sole traders |
| REA number | Province code + numeric (e.g. MI-1234567) | Registro Imprese economic administrative index; uniquely identifies a company |
| Numero di iscrizione | Numeric | Company registration number at the Registro Imprese |

For individuals: CF encodes surname initials, name initials, DOB, and birth municipality.
Use CF to disambiguate same-name individuals in registry searches.

---

## Corporate Registry: Registro Imprese

**URL:** registroimprese.it  
**Access:** Free basic search; paid documents

### What it contains
- Full corporate record for every Italian entity (SRL, SPA, SNC, associazioni, etc.)
- Incorporation date, registered address (sede legale), registered capital (capitale sociale)
- Current and historic officers: amministratori, soci, revisori, liquidatori
- Filed annual accounts (bilanci): income statement, balance sheet, notes
- Articles of incorporation and amendments
- UBO (titolare effettivo) declarations under D.Lgs. 231/2007
- Liens, mortgages, and encumbrances
- Procedures (liquidation, bankruptcy, insolvency)

### Key document types and costs (approximate)

| Document | Italian name | Cost | Unlock value |
|---|---|---|---|
| Company profile summary | Visura camerale ordinaria | €8-12 | HIGH: full officer list, capital, registered address, REA |
| Full filing history | Visura storica | €10-15 | HIGH: historic officers, all amendments |
| Filed accounts | Bilancio d'esercizio | €3-8 per year | HIGH: revenue, EBITDA, net result, equity |
| Articles of incorporation | Atto costitutivo | €5-10 | MEDIUM: ownership structure at founding |
| UBO declaration | Dichiarazione titolare effettivo | €5-10 | HIGH: beneficial owner(s) by name |
| Liens and mortgages | Visura ipotecaria / protesti | €5-10 | MEDIUM: financial stress indicators |

**Single best document for most investigations:** Visura camerale ordinaria (€10).
Closes identity anchoring, financial ghost, address ghost, and officer mapping in one pull.

### Free alternatives
- InfoCamere public search (infocamere.it): free company name lookup, basic details
- ATECO code search: find all companies in a sector and municipality (free)
- PEC registry lookup (INI-PEC): find entity's certified email address (free)

---

## Bankruptcy and Insolvency

**URL:** fallcivile.giustizia.it (historic) and Portale dei Servizi Telematici (current)  
**Also:** Albo dei Curatori Fallimentari (register of bankruptcy trustees)

Search by entity name or CF. Italian bankruptcy proceedings (fallimento, now
liquidazione giudiziale under D.Lgs. 14/2019) are public. Concordato preventivo
(restructuring) is also searchable.

Free summary search available. Full proceedings require registration.

---

## Civil and Commercial Litigation

**URL:** Portale dei Servizi Telematici — pst.giustizia.it  
**Access:** Free search by party name; full case access requires attorney registration

Covers civil proceedings in tribunali ordinari. Useful for: debt recovery cases,
corporate disputes, injunctions, contract litigation.

Limitation: criminal proceedings are not publicly indexed except for final judgments
published in the official gazette (Gazzetta Ufficiale) in specific circumstances.

---

## PEP and Political Office

**Parlamento Italiano:** parlamento.it — searchable by name for members of parliament
(Camera and Senato), including historic terms. Lists committee memberships and declared
interests.

**Governo Italiano:** governo.it — current ministers and undersecretaries.

**Regione / Comune:** regional and municipal council members are listed on their
respective institutional websites. No unified national registry.

**Published UBO and PEP lists:**
- Consob (securities regulator): consob.it — listed company insiders, major shareholders
- Banca d'Italia: bancaditalia.it — bank officer fit-and-proper records (limited public access)
- MEF (Ministry of Economy): mef.gov.it — public company officer disclosures

---

## Professional Licensing and Registers

| Profession | Registry | URL |
|---|---|---|
| Avvocati (lawyers) | Consiglio Nazionale Forense | ordineavvocati.it (each bar) |
| Commercialisti (accountants) | CNDCEC | commercialisti.it |
| Notai (notaries) | Consiglio Nazionale del Notariato | notariato.it |
| Medici (doctors) | FNOMCeO | fnomceo.it |
| Ingegneri (engineers) | CNI | cnipe.it |
| Architetti (architects) | CNAPPC | archiworld.it |

Professional registers are searchable by name. Disbarment and disciplinary
proceedings may or may not be public depending on the order.

---

## Sanctions and AML

**Banca d'Italia AML lists:** bancaditalia.it/compiti/vigilanza/aml
**OFAC SDN list:** ofac.treas.gov (US persons and targets)
**EU Consolidated Sanctions List:** eeas.europa.eu/topics/sanctions-policy
**UN Security Council Sanctions:** un.org/securitycouncil/sanctions/information

Italian entities must comply with all four. Search all four for any Italian target
with international exposure.

---

## Domain and Infrastructure

**WHOIS for .it domains:** nic.it (free lookup)  
**Certification Authority filings:** agid.gov.it (for entities with qualified electronic signatures)  
**ATECO code lookup:** istat.it/it/strumenti/definizioni-e-classificazioni/ateco-2007

---

## Investigative Media and Leaks

**Italian investigative journalism:**
- Il Fatto Quotidiano (ilfattoquotidiano.it) — corporate and political investigations
- L'Espresso (espresso.repubblica.it) — long-form investigative
- Domani (editorialedomani.it) — financial and political investigations

**ICIJ Italian connections:** icij.org — search Offshore Leaks, Panama Papers,
Pandora Papers databases by name

**OpenCorporates Italy:** opencorporates.com — aggregates Registro Imprese data;
useful for free structural mapping before paying for documents

---

## Search Technique Notes

**Language:** Run all searches in Italian first. Corporate names, officer names,
and press coverage may appear differently in Italian vs. English. Use "site:it"
Google filter for Italian-domain results.

**Name disambiguation:** Italian surnames are often regional. If the name is common
(e.g. Rossi, Ferrari, Bianchi), the Codice Fiscale is the only reliable anchor.
Without it, cross-reference DOB and birth municipality from any available source.

**SRL vs SPA:** Most Italian startups and SMEs are SRL (società a responsabilità
limitata). SPA (società per azioni) is for larger entities. Key difference for
OSINT: SRL minimum capital is €10,000 (often just €1 for SRL semplificata); SPA
requires €50,000 minimum. Very low registered capital in a supposedly funded SRL
is a capital ghost signal.

**Domicilio fiscale providers:** common for early-stage Italian companies. Known
providers include addresses in Milan (Via Torino, Via Dante areas), Rome (EUR area),
and known coworking addresses. >10 companies at one address = virtual office.

**Quote sociali:** In an SRL, ownership shares are "quote sociali" not "azioni".
The cap table is in the visura. Transfers of quote require a notarised act.

---

## Cost-Optimised Unlock Sequence for an Italian Target

**€0 budget:**
1. Registro Imprese free search: company name, incorporation date, REA
2. OpenCorporates: officer list snapshot
3. Google.it: name + "Registro Imprese" + "bilancio" + "socio"
4. ICIJ Offshore Leaks database
5. Portale dei Servizi Telematici: civil litigation check

**€10-15 budget (after free pass):**
6. Visura camerale ordinaria — closes officer history, capital, address
7. Most recent bilancio d'esercizio — closes financial ghost

**€25-50 budget:**
8. Visura storica — closes historic officer mapping
9. UBO declaration — closes beneficial ownership ghost
10. 2-3 years of bilanci — establishes financial trajectory

**€100+ budget:**
11. Full bilancio package (5 years)
12. Portale dei Servizi Telematici attorney access (requires registration)
13. PitchBook / Crunchbase Pro for funding round verification
14. Cerved or CRIBIS credit profile


## Worked Example: Italian Corporate Target

Degrees of Freedom

| Element | Tight (no deviation) | Flexible (judgment) |
|---|---|---|
| Contamination Firewall | PRIVATE never enters findings. INFERENCE always labelled. | How much INFERENCE synthesis to include per section |
| Source Confidence Tensor | Every PUBLIC entry scored on all 5 flags | How prominently to display low-confidence entries |
| Ethics gate | Always runs. Flag is visible if triggered. | Operator override accepted with noted context |
| Ghost Inventory | Full 12-type taxonomy checked | How many ghosts to surface vs. suppress as noise |
| Paywall Ledger | Always produced. Single best document always named. | Depth of alternative-source suggestions |
| Operational Triage | Always three tiers. Always costed. Always time-boxed. | Specific actions per tier (judgment call) |
| PEP Map | Always produced when PEP detected | Depth of Degree 2 mapping |

---

## Worked Example

**Operator:** "Investigate Claudio Pandini, Italian, connected to a startup called Cantieri Digitali MedTech. No fiscal code."

**Skill output (condensed):**

**Identity anchors:** Claudio Pandini (unverified DOB), Italian national. Entity: Cantieri Digitali MedTech SRL. No fiscal code provided — disambiguation risk; will flag if common name.

**Phase 2 (Structural):** Registro Imprese search for "Cantieri Digitali MedTech" — [search results, source-tagged PRIMARY]. Pandini directorship — [confirmed / PRIMARY / CORROBORATED / INDEPENDENT / FRESH / THIRD-PARTY].

**Ghost Inventory:** Financial ghost — no accounts filed beyond incorporation year. Address ghost — registered office is a domicilio fiscale provider. Capital ghost — press announced a seed round; no cap table change visible in Registro Imprese filing.

**Paywall Ledger:** Visura ordinaria (€10, Registro Imprese) — full officer history, filed accounts, capital changes. **Single best document: visura ordinaria. Closes financial ghost and capital ghost in one document.**

**Operational Triage:**
> €0 / 1 hour: Search "Claudio Pandini" + "Cantieri Digitali" on LinkedIn, Google, and WHOIS for domain registration. Maps public claims vs. structural record.
> €10 / 2 hours: Pull visura ordinaria from Registro Imprese. Closes financial, address, and capital ghosts.
> €100 / 1 day: Visura + PitchBook Italia search + Italian court filing index (Portale dei Servizi Telematici). Closes 90% of open questions.

---

## Boundaries

This skill explicitly does not:

- Access paid databases or closed government systems
- Produce surveillance plans, physical location inference, or real-time tracking recommendations
- Replace qualified legal, compliance, or domain expert advice
- Carry findings between sessions
- Merge PRIVATE project metadata into PUBLIC evidence (contamination firewall)

Adjacent skills:
- `tool--doc-safety-analysis`: for extracting from a closed document set
- `fun--fin-dexter-deep-research`: for financial filing analysis and earnings data
- `tool--interview-analyst-and-compliance-check`: for interview transcript cross-referencing
