# PaidHR ICP

Built only from `context/docs/research/`. Each row cites the research file it comes from (for example "company.md" means `context/docs/research/company.md`). INFERRED means the research suggests it but does not state it. Last updated 2026-11-12.

## Layer 1: Firmographic

| Attribute | Core ICP | Secondary | Source |
|---|---|---|---|
| Geography | Nigeria, Lagos first ("Our core market is still Nigeria") | Kenya, Ghana, South Africa (existing but smaller) | company.md (Markets) |
| Headcount | 20 to 200 employees | 100+ ("Enterprise" tier, "fast-growing and enterprise organizations") | company.md (Pricing); customers.md (Customer profile) |
| Industry | **Unproven. Test two hypotheses, see "Industry: evidence check" below.** H1: funded tech and fintech startups (8 of 13 named customers we can classify). H2: logistics, retail, manufacturing (one investor sentence, 2 of 13 named customers) | Engineering and energy (AEC, Sygnite); research (NOI Polls) | customers.md; customer-videos.md |
| Footprint | Multi-state in Nigeria, or staff in more than one African country | Diaspora or foreign-currency-paid staff | signals.md (A2, A3); market.md (1) |
| Workforce type | Shift, field or frontline staff who do not sit at a desk (INFERRED from product launches, not from any named customer; goes with H2) | Office teams (fits H1, the segment our named customers come from) | company.md (Direction); signals.md (A3) |

### Industry: evidence check

The previous version made logistics, retail and manufacturing the core and put tech/fintech as secondary. The evidence points the other way, or at best is split.

| Evidence | Says | Weight | Source |
|---|---|---|---|
| Accion seed release, June 2025 | "The majority of PaidHR's users are logistics, retail, and manufacturing businesses with under 200 employees" | One sentence in an investor press release. No customer names, no counts, no share given | customers.md (Customer profile) |
| 13 named customers with a known industry | Tech/fintech 8 (Risevest, Mono, Flutterwave, GetEquity, Kredete, GO54, Mira, FAMASI). Logistics 1 (Heroshe). Food processing 1 (ReelFruit). Engineering/energy 2 (AEC, Sygnite). Research 1 (NOI Polls). Retail 0 | What PaidHR chose to show publicly. Logo pages, last updated 2025 | customers.md (Named customers); customer-videos.md |
| 6 homepage logos (Wilan, Waaw Foundation, Selar, Redeemer Health Village, Nafa, Gasland) | Industry not researched | Unclassified | customers.md (Homepage logo strip) |
| Customer count by industry | unknown | No data | customers.md (Gaps) |

What this means for sales (INFERRED):
- **H1 (tech/fintech)** is the only segment where we can name peers in copy and on calls. Weakness: these buyers are the most likely to already use a competitor or a modern tool.
- **H2 (logistics, retail, manufacturing)** fits the product direction (WhatsApp/USSD, geo-referenced attendance) and the "still on spreadsheets" story. Weakness: we can reference only Heroshe (2023, size unknown) and ReelFruit (logo only). No retail customer is named anywhere.
- Until the CRM split by industry is known, run both as separate list tests with the same volume and compare reply and meeting rates. Do not write "most of our customers are in logistics, retail and manufacturing" in prospect copy: the only source is Accion, and the named customers do not show it.
| Channel partners | Payroll agencies, HR consultants, accounting firms running client payroll (white-label) | | customers.md; company.md (Modules) |

## Layer 2: Technographic

| Current setup | Why it fits | Source |
|---|---|---|
| Spreadsheets plus bank files emailed back and forth | PaidHR: "Your days of back and forth emailing of spreadsheets and bank files are over!" Accion: logistics, retail and manufacturing firms "still rely on spreadsheets or manual processes" | voice.md; customers.md |
| Statutory remittances done by hand, one agency at a time | FAMASI and Heroshe both describe this before Pade | customer-videos.md |
| Payroll engine still on pre-2026 PAYE bands | DLA Piper: systems on old bands give wrong deductions | market.md (1) |
| Several disconnected HR tools | PaidHR: "People operations without the patchwork" | voice.md |
| An ERP or finance system they want to keep | Payroll API, journals pushed to the ERP ("Your ERP stays") | company.md (Integrations) |
| Former Bento Africa customer | Bento stopped processing payroll in Feb 2025 | competitors.md; signals.md (A2) |

## Layer 3: Intent signals

| Signal | Strength | Source |
|---|---|---|
| Next annual PAYE return due 31 Jan 2027 (first under the new bands) | High, dated | signals.md (A1); market.md (1) |
| PenCom pension defaulter exposure; PCC now blocked until penalties are paid | High | market.md (4); signals.md (A2) |
| Crossing 20 employees | Medium | signals.md (A3) |
| Hiring a first HR manager or payroll officer | Medium (INFERRED) | signals.md (A3) |
| Hiring in a second African country | Medium | signals.md (A3) |
| Recently raised funding | Medium (INFERRED) | signals.md (A3) |
| Cash-flow gap at payroll time (fits Zitra financing) | Medium | signals.md (A3) |
| Medium-taxpayer e-invoicing enforcement, Jan to Mar 2027 | Low (INFERRED, not a payroll rule) | market.md (3) |
| Minimum wage review (timing disputed) | Low until a figure is agreed | market.md (6) |

## Layer 4: Persona

| Role | Buying role | Detail |
|---|---|---|
| HR manager / HR operations lead | Champion | personas.md |
| Founder/CEO (SME) or CFO / Head of Finance (100+) | Economic buyer | personas.md |
| Finance or accounts lead who funds and reconciles payroll | Influencer | personas.md |

## Layer 5: Engagement readiness

| Tier | Definition | Source |
|---|---|---|
| Ready now | In ICP, plus a dated compliance trigger (Jan 31 return, PenCom exposure) or a payroll failure (ex-Bento) | signals.md (A1, A2) |
| Warming | In ICP, plus a growth trigger (new HR hire, funding, new country, crossing 20 staff) | signals.md (A3) |
| Nurture | In ICP, no trigger seen. Use content assets (PaidRight salary report, tax-law guides) | signals.md (Part B); voice.md |

## Not a fit

| Who | Why | Source |
|---|---|---|
| Businesses outside Africa | PaidHR markets only to African businesses | company.md |
| Firms that want payroll fully run for them (outsourced or EOR) | Workpay sells EOR and outsourced payroll. PaidHR's research shows software plus automated remittance, not a managed service (INFERRED) | competitors.md (Workpay) |
| Price-only buyers looking for a free tool | Salario offers free payroll for Nigerian SMEs | competitors.md (Salario) |
| Finance teams that want payroll inside a spend tool | Bujeti sells payroll to finance, inside expenses (INFERRED) | competitors.md (Bujeti) |

## Open questions
- **Top priority: what is the real customer split by industry and headcount?** Needs a CRM or billing export from PaidHR (not in the research). This decides whether H1 or H2 is core. Until then both are hypotheses. (customers.md, Gaps)
- Is under 20 employees a fit? No tier is published below 20, but FAMASI was founder-run (size unknown). (company.md; customer-videos.md)
- Which segment converts best: SMEs (2025 focus) or enterprise (2026 focus)? (company.md, Direction)
- Win rates by industry are unknown.
