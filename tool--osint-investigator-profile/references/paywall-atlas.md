# Paywall Atlas

Indexed paid sources for OSINT investigations, by jurisdiction and target type.
Each entry: source, what it contains, cost tier, unlock value, free alternative.

Cost tiers: LOW (<€20), MEDIUM (€20-100), HIGH (€100-500), ENTERPRISE (>€500/mo subscription).

---

## Italy

### Corporate and Registry

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| Registro Imprese — visura ordinaria | Full officer list, capital, address, REA | LOW (€10) | HIGH | OpenCorporates snapshot (partial) |
| Registro Imprese — visura storica | Historic officers, all amendments | LOW (€12) | HIGH | None |
| Registro Imprese — bilancio singolo | One year filed accounts | LOW (€5) | HIGH | None |
| Registro Imprese — bilancio 5 anni | Five years filed accounts | LOW (€20) | HIGH | None |
| Registro Imprese — UBO declaration | Beneficial owner by name | LOW (€8) | HIGH | None |
| Cerved Group | Credit risk profile, payment history, financial ratios | MEDIUM (€50-80) | HIGH | Cerved free basic search |
| CRIBIS | Credit score, payment behaviour, protest history | MEDIUM (€40-70) | HIGH | CRIBIS free basic |
| Orbis (Bureau van Dijk) | Global corporate ownership chains, financials | ENTERPRISE | HIGH | OpenCorporates |
| PitchBook | VC/PE funding rounds, cap tables, investors | ENTERPRISE | HIGH for funded startups | Crunchbase free tier |
| Crunchbase Pro | Funding rounds, investors, news | MEDIUM (€30/mo) | MEDIUM | Crunchbase free tier |

### Litigation and Legal

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| Portale dei Servizi Telematici (attorney access) | Full civil case filings | MEDIUM | HIGH | Free name search (limited) |
| Eurisc (CRIF) | Consumer credit history (for individuals) | LOW (€15 self-request) | MEDIUM | None (operator must be the subject) |

### Media and Investigation

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| Factiva (Dow Jones) | Italian and global press archive | ENTERPRISE | HIGH | Google News (limited) |
| LexisNexis | Legal and news database, Italian press | ENTERPRISE | HIGH | Google News |

---

## United Kingdom

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| Companies House | All UK company filings, officers, PSC register | FREE | HIGH | — |
| HMRC VAT register | VAT registration (limited) | FREE | LOW | — |
| Land Registry | Property ownership | LOW (£3 per title) | MEDIUM | None |
| Courts and Tribunals Judiciary | Civil judgment register | FREE | MEDIUM | — |
| Insolvency Service | Bankruptcy and disqualification | FREE | HIGH | — |

---

## United States

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| SEC EDGAR | All public company filings (10-K, 8-K, proxy) | FREE | HIGH | — |
| PACER | Federal court dockets | LOW (per page) | HIGH | CourtListener (limited free) |
| State SOS registries | Entity registration by state | FREE (most states) | HIGH | OpenCorporates |
| Dun & Bradstreet | Credit and business intelligence | ENTERPRISE | HIGH | LinkedIn (partial) |
| LexisNexis Accurint | People search, address history | ENTERPRISE | HIGH | None (requires professional account) |
| Pipl / BeenVerified | People search aggregator | MEDIUM | MEDIUM | Spokeo (limited) |

---

## European Cross-Border

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| e-Justice Portal (EU) | Corporate registers across EU member states | FREE | HIGH | — |
| OpenCorporates | 200+ jurisdictions, aggregated | FREE (basic) / MEDIUM (API) | HIGH | — |
| Orbis (Bureau van Dijk) | Global corporate ownership, financials | ENTERPRISE | HIGH | OpenCorporates |
| World-Check (Refinitiv) | Global PEP and sanctions | ENTERPRISE | HIGH | UN, EU, OFAC free lists |
| Dow Jones Risk & Compliance | PEP, sanctions, adverse media | ENTERPRISE | HIGH | Free sanctions lists |

---

## Offshore and Secrecy Jurisdictions

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| ICIJ Offshore Leaks DB | Panama Papers, Pandora, FinCEN Files, Offshore Leaks | FREE | HIGH (where covered) | — |
| BVI Registry | Entity search (limited) | FREE (basic) | LOW | — |
| Cayman CIMA | Regulated entity search | FREE | LOW | — |
| OpenOwnership | Beneficial ownership data (countries with public registers) | FREE | HIGH (where covered) | — |

---

## Global People Search

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| LinkedIn Sales Navigator | Full LinkedIn profile access, company insights | MEDIUM (€80/mo) | HIGH | LinkedIn free (limited) |
| Hunter.io | Email pattern discovery for corporate domains | LOW (freemium) | MEDIUM | LinkedIn |
| Spokeo | US people search | LOW | LOW-MEDIUM | None |
| Pipl | Global people search, deep web | MEDIUM | MEDIUM | None |

---

## Domain and Infrastructure

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| DomainTools | WHOIS history, reverse WHOIS by registrant | MEDIUM (€90/mo) | HIGH | Historic WHOIS via Wayback Machine |
| Shodan | Internet-facing infrastructure, open ports | LOW (€50 lifetime) | MEDIUM | Censys (limited free) |
| BuiltWith | Technology stack of websites | LOW (freemium) | LOW | Wappalyzer |
| SecurityTrails | DNS history, subdomain enumeration | MEDIUM | MEDIUM | PassiveDNS (limited free) |

---

## Procurement and Public Contracts (Italy)

| Source | Contains | Cost | Unlock Value | Free Alternative |
|---|---|---|---|---|
| ANAC (Autorità Nazionale Anticorruzione) | Public procurement database, supplier lists | FREE | HIGH for public-sector adjacent targets | — |
| MePA (Mercato Elettronico PA) | Public administration marketplace suppliers | FREE | MEDIUM | — |
| Siope+ / RGS | Public expenditure data | FREE | MEDIUM | — |

---

## Usage Notes

**Cost-optimisation rule:** for Italian targets, visura ordinaria + most recent bilancio
closes 70% of standard investigation questions for €15. Add visura storica and UBO
declaration for €30 total to close 90% of structural questions.

**Enterprise subscriptions:** Orbis, LexisNexis, World-Check, and Factiva are designed
for institutional buyers (law firms, compliance teams, banks). Individual access is
possible but priced for organisations. If you need one search, check whether a
provider offers a one-off purchase option before subscribing.

**ICIJ databases are always free:** The Offshore Leaks database at icij.org/investigations/
covers Panama Papers, Paradise Papers, Pandora Papers, FinCEN Files, and Offshore
Leaks. Search by person name or entity name. High unlock value at zero cost for
any target with offshore exposure.

**For Italian funded startups:** Registro Imprese + Crunchbase free + LinkedIn company
page gives a reasonable picture. PitchBook is only worth paying for if the target
has raised >€5M and you need verified cap table data.
