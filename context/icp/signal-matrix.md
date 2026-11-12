# PaidHR Signal Matrix

**Status: DRAFT, work in progress.** Segments confirmed by Fredrick on 2026-11-12 (4 rows). Angles, priorities and signals still need review with PaidHR sales and leadership. Built with the `signal-matrix` skill from `knowledge-base/playbooks/signal-matrix-framework.md` and `workflow-anatomy.md`. Last updated 2026-11-12.

**How to read this file**
- Facts come from `context/docs/research/` (cited by file name, e.g. "signals.md" means `context/docs/research/signals.md`), from `context/icp/`, or from web searches and treg catalog lookups made on 2026-11-12 (cited by URL or endpoint).
- **INFERRED** means the research suggests it but does not state it.
- Volumes are not estimated with numbers: no source gives them. Every volume score is INFERRED and must be checked with a small test pull before a workflow is built.
- treg prices are per call, from `catalog_search` on 2026-11-12. Clay credit costs are **unknown** until the table is designed (`campaign-contract`, `calculate-costs`).
- No angle below is approved copy. Every claim used in copy must come from `context/messaging/facts.md`, where all claims are still Pending approval.

---

## Matrix 1: Segments

### Why these rows
The ICP review (icp.md, "Industry: evidence check") left industry as two untested hypotheses: H1 tech/fintech and H2 logistics, retail and manufacturing. They are separate rows so the same signal can run in both and be compared. Size splits off the 200+ employers PaidHR is moving towards (company.md, Direction). Payroll partners are a different buyer for a different product (white-label), so they get their own row.

Cross-border staff and ex-Bento customers are **signals**, not rows: they cut across segments.

| Rule | How the rows meet it |
|---|---|
| Observable | Industry and headcount come from LinkedIn/Apollo company data; partners from website text and directory listings |
| Mutually exclusive | S1 and S2 are 20 to 199 staff and split by industry. S3 is 200+ staff, any industry. S4 is any firm whose service is running payroll for clients |
| Actionable | Each row has a different lead product and a different first conversation (see angles) |
| Volume | Hundreds or more per row is INFERRED, not counted. S4 is the smallest (INFERRED) |

### Segment definitions

| # | Segment | Target definition (filterable) | Real examples (not yet customers; checked 2026-11-12) | Lead product / upsell (hypotheses) | Priority |
|---|---|---|---|---|---|
| S1 | Funded tech and fintech (H1) | Nigeria HQ; 20 to 199 employees; LinkedIn industry in Software, Financial Services, IT Services or Internet; at least one disclosed funding round. Headcount band from Apollo/LinkedIn | Carrot Credit, Umba (Seedtable Lagos ranking); Nectarfi ($170k pre-seed, Apr 2026); Bfree (largest Nigerian deal, Apr 2026, per Nairametrics). Headcount not checked for any of them. Lookalike seeds from current customers: Mono, Risevest, Kredete, GetEquity, GO54 (customers.md) | Payroll + statutory remittance / cross-border payroll, EWA | **High.** The only segment where PaidHR can name peers (8 of 13 classifiable named customers, icp.md). Best data coverage |
| S2 | Frontline operators (H2) | Nigeria HQ; 20 to 199 employees; LinkedIn industry in Transportation/Logistics, Retail, Food and Beverage Manufacturing, Manufacturing, Warehousing; shift, field or store staff (frontline job posts, see N1) | Logistics: JAM-Forte Logistics, TL First Group, Fleximotion, 3F Logistics & Services, Travo, Telcapit (Lagos rider and fleet job posts, Jobberman / HotNigerianJobs, mid to late 2026). Retail: Prince Ebeano, Justrite, Everyday Supermarket (multi-branch supermarket chains). Headcount not checked; the supermarkets may be 200+ (then S3) | Time & Attendance + payroll / EWA and Loan Marketplace, WhatsApp/USSD access | **High, as a test.** Accion says most users are here (customers.md) and the 2026 product moves point here (company.md, Direction). Proof is thin: only Heroshe and ReelFruit are named |
| S3 | Large Nigerian employers | Nigeria HQ or main operation; 200+ employees, any industry; a finance function (CFO or Financial Controller on LinkedIn); offices or branches in 2+ states | Marketsquare (36 branches in 12 states, BusinessDay); SPAR Nigeria / Artee Group (stores in Lagos, Abuja, Port Harcourt, Enugu, Calabar); Interswitch, Nomba, Moove (Seedtable 2026 Lagos ranking). Headcounts not checked | Payroll + remittance with ERP journals ("Your ERP stays") / Performance (Balanced Scorecard), EWA | **Medium.** The CEO is hiring enterprise sales (signals.md, Part B), but there is no named 200+ case study and SeamlessHR runs named enterprise proof (competitors.md). Who signs (CEO or CFO) is unknown (personas.md) |
| S4 | Payroll partners | Firms in Nigeria whose website or directory listing offers payroll administration, payroll outsourcing or staffing/EOR to clients: HR consultancies, staffing firms, accounting firms. Any size | Rovedana Limited (staffing and payroll BPO, Yaba); Oxus HR & Payroll (Ikeja); PoweredUp Consulting (Ogudu); Growth Partner Intermediaries (Lagos, founded 2023); Phillips Outsourcing, HumanManager (Lekki); SOL Nigeria (payroll admin and EOR, 12 offices); BDO Nigeria payroll services; Benold Resources, Modessa Consulting (accounting firms). Sources: Clutch, ConnectCiti, firm sites | White-label payroll / Payroll API / EWA for their clients' staff (INFERRED) | **Medium.** One partner runs many client payrolls (INFERRED). PaidHR sells to them openly ("Launch your own Payroll business today!", voice.md). No partner customer is named and partner pricing is unknown |

**Not rows, and why**
- **Under 20 staff:** no published tier below 20 and Salario gives payroll away free (company.md; competitors.md). Open question in icp.md.
- **Kenya, Ghana, South Africa HQ:** no regulation or customer research for these markets (market.md, section 9). Revisit after Nigeria works.
- **Firms wanting fully managed payroll or EOR:** Workpay's ground; not a PaidHR fit (icp.md, Not a fit). Note: S4 firms that *sell* EOR are partners, not buyers of EOR.

### Angles by segment and persona

Personas from personas.md. For S4 the same three roles map to the partner firm: the payroll practice lead (champion), the owner or managing partner (economic buyer) and the tax or compliance lead (influencer). Angles describe the buyer's situation, not a diagnosis of their problem (framework guardrail).

| Segment | Champion: HR manager / HR ops | Economic buyer: Founder/CEO (SME) or CFO (200+) | Influencer: Finance or accounts lead |
|---|---|---|---|
| **S1 Funded tech/fintech** | Every remittance (PAYE, pension, NSITF) filed from one run, with a receipt for each as "evidence for the payment", and staff who see their own payslips instead of asking HR. From Heroshe's own words (customer-videos.md) | Payroll stops taking a slot in the founder's week. FAMASI's CEO got back "that one hour" per run (customer-videos.md). With staff in other countries: one payroll in 45+ currencies (company.md; the 45 vs 49 figure conflicts, use neither until facts.md approves one) | Deductions on the 2026 PAYE bands, and foreign-currency pay taxed at the official CBN rate, which the Tax Act now requires (market.md, section 1). Journals exported, not retyped |
| **S2 Frontline operators** | Shifts and hours from geo-tracked attendance flow straight into payroll ("Every hour tracked, every hour paid", voice.md). Riders and store staff check payslips and request leave on WhatsApp or USSD, no app needed (company.md) | Paying field staff correctly across branches without a payroll clerk per site (INFERRED). Earned wage access as a retention tool at "no extra cost to your business" (voice.md; PaidHR claim, Pending in facts.md) | Pension due within 7 working days of salary, and PenCom now blocks the Pension Clearance Certificate until penalties are paid (market.md, section 4). Multi-state PAYE remitted to each state (signals.md, A2) |
| **S3 Large employers** | One record for HR, payroll, attendance and performance instead of separate tools ("People operations without the patchwork", voice.md). Balanced Scorecard for appraisal season, which competitors are campaigning on in Oct-Nov 2026 (signals.md, A3) | Compliance exposure at scale: PenCom auto-calculates penalties and blocks the PCC; NTAA 2025 widens employer PAYE liability (market.md, sections 1 and 4). "Your ERP stays" (voice.md) | Payroll journals pushed into the ERP; no named ERP integration exists yet, so do not name one (company.md, Integrations). Wrong deductions on old-band systems (market.md, section 1) |
| **S4 Payroll partners** | Run every client's payroll and remittances from one place instead of one spreadsheet per client (INFERRED from "Your days of back and forth emailing of spreadsheets and bank files are over!", voice.md) | Sell payroll under their own brand: "Make our payroll technology yours" / "Let us handle the backend, while you take the credit" (voice.md, white-label page) | Every client moved to the 2026 bands, NHF now voluntary, and rent relief documentation, handled once in the engine instead of client by client (market.md, section 1) |

### Phase 2: assessment

| Segment | Signal density | Product-market fit | Penetrability (Nigeria data) | Combined |
|---|---|---|---|---|
| S1 Funded tech/fintech | High: hiring, funding, headcount, cross-border hiring all apply | High for payroll and remittance; also the most likely to already use a competitor (INFERRED) | High: these firms are on LinkedIn, Crunchbase-type sources and job boards | **1st** |
| S2 Frontline operators | Medium: frontline hiring and Google Maps locations apply; funding does not | High on paper (T&A, WhatsApp/USSD, EWA), unproven in named customers | Medium: company pages exist, but frontline staff are mostly not on LinkedIn, so headcount is undercounted (INFERRED) | **2nd** |
| S4 Payroll partners | Low to medium: website text plus their own payroll hiring | Medium: product exists, no named partner | High for finding them (directories, sites); contacts are small firms (INFERRED) | **3rd** |
| S3 Large employers | Medium: leadership, ERP mentions, multi-state | Medium: the pitch exists, the proof does not | High for company data; buyer unclear | **4th** |

---

## Matrix 2: Signals

### Scoring
Each signal is scored 1 to 5 on the four tests from `workflow-anatomy.md` (Choosing a Mother Intent):
- **Reason:** clear buying reason. Does it explain why they would need PaidHR now?
- **Volume:** enough volume to sustain campaigns (INFERRED for every signal; no counts exist).
- **Data:** can we detect it reliably with a named tool today?
- **Story:** a natural, non-creepy line in the opener.

Status: VERIFIED (tool exists and the signal is unambiguous), VERIFIED (COMPOUND) (only useful stacked on another signal), VERIFIED (PARTIAL) (works for some segments), HOLD, or KILLED.

"Verified" here means the data source and tool exist in treg's catalog. No test pull has been run, so Nigerian coverage of each provider is **unknown** and must be checked first.

### Universal signals (one build, keyword swaps per segment)

| ID | Signal | Mother intent | Segments | Data source | Detection tool (price per call) | Key filters | Reason | Volume | Data | Story | Total /20 | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| U1 | Hiring an HR manager, HR officer, payroll officer or People Ops lead | Hiring specific roles | S1, S2, S3, S4 (S4: payroll officers for client work) | Job boards: LinkedIn Jobs, Indeed, Jobberman (Jobberman listings seen in search, 2026-11-12) | treg `companies.jobs.search` (routed; blitzapi $0.001); `apollo.companies.jobs` for known accounts ($0.026 per company, signals.md A4); Clay "Find Jobs" | Titles: HR Manager, HR Officer, HR Generalist, Payroll Officer, Payroll Accountant, People Operations. Location Nigeria. Posted in the last 30 days | 4 | 4 | 3 | 5 | **16** | VERIFIED. Coverage of Jobberman by these providers unknown |
| U2 | That job post asks for manual payroll skills: Excel payroll, PAYE or pension computation, remittance to agencies | Hiring specific roles (description keywords) | S1, S2, S3 | Job description text from U1 | Same as U1, filter on description; or Clay AI column on the description | "Excel", "payroll computation", "PAYE", "pension remittance", "NSITF", "statutory deductions" | 5 | 2 | 3 | 4 | **14** | VERIFIED (COMPOUND). Stack on U1. Story must describe the role, not diagnose ("the role covers PAYE and pension remittance") |
| U3 | New HR leader or finance leader started in the last 90 days | New leadership | S1, S2, S3 | LinkedIn profile start dates | treg `apollo.people.search` ($0); `leadmagic.x.job-change-detector` ($0.075) to confirm; Clay "Find People" | Titles as U1 plus CFO, Finance Manager, Financial Controller; start date in the last 90 days; company in ICP | 3 | 3 | 3 | 4 | **13** | VERIFIED. Reason INFERRED ("a new HR leader typically re-evaluates tools in their first 90 days", signals.md A3) |
| U4 | Headcount up 20%+ in 6 months, or crossing 20 employees | Hypergrowth (headcount) | S1 (S3 weaker; S2 undercounted) | LinkedIn headcount history | treg `companyenrich.companies.workforce` ($0.049); `linkedin.company.profile` (crawl4ai $0.0009) monthly snapshots | Delta over 6 months; or band moving from 11-50 to 51-200 | 3 | 3 | 3 | 3 | **12** | VERIFIED (PARTIAL). Not for S2: frontline staff are rarely on LinkedIn (INFERRED) |
| U5 | Raised a funding round in the last 6 months | Fundraising | S1 only | Funding databases; Nairametrics and TechCabal monthly deal round-ups | treg `aviato.companies.funding_rounds` ($0.01); `predictleads.companies.financing_events` ($0.04) | Nigeria HQ; round date in the last 180 days; seed to Series B | 3 | 2 | 4 | 4 | **13** | VERIFIED for S1. Volume is low: Nairametrics counted six major deals in April 2026. KILLED for S2, S3, S4 (not VC-funded markets, framework rule) |
| U6 | Nigeria-based company hiring in a second African country | Hiring offshore | S1, S3 | Job posts whose location is Kenya, Ghana or South Africa, at a Nigeria HQ company | Same as U1, location filter | Company HQ Nigeria; job location KE, GH, ZA; last 60 days | 4 | 2 | 3 | 5 | **14** | VERIFIED. Maps to cross-border payroll (company.md, Modules) |
| U7 | Engaging with LinkedIn posts on the 2026 tax reform, PAYE or payroll, including competitor posts | Content engagement | S1, S2, S3 | LinkedIn posts by SeamlessHR, Workpay, Bujeti, Salario, PaidHR and Nigerian tax firms | treg `linkedin.search.posts` (anyapi $0.002) then `linkedin.post.reactions` (fetchinio $0.0015); the `lead-signals` skill | Keywords: "PAYE", "Nigeria Tax Act", "payroll", "pension remittance"; engager's title HR or finance, company in ICP | 3 | 3 | 4 | 2 | **12** | VERIFIED (COMPOUND). Do not cite the engagement in copy; use it to route and time |
| U8 | Lookalike of a named customer | Lookalike of customers | S1, S2 | Seed lists: S1 Mono, Risevest, Kredete, GetEquity, GO54; S2 Heroshe, ReelFruit (customers.md) | Clay lookalike; treg company search by industry and size | Same industry and headcount band as the seed | 2 | 4 | 4 | 3 | **13** | VERIFIED (COMPOUND). No timing on its own. Naming customers in copy needs approval (facts.md, all Pending) |

### Niche-specific signals (separate build per segment)

| ID | Signal | Mother intent | Segments | Data source | Detection tool (price per call) | Key filters | Reason | Volume | Data | Story | Total /20 | Status |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| N1 | Frontline hiring velocity: 3+ posts for riders, drivers, warehouse, store or field sales staff in 60 days | Hiring velocity | S2 | Jobberman, HotNigerianJobs, Jobzilla, LinkedIn Jobs (rider posts at JAM-Forte, TL First Group, Fleximotion, 3F Logistics seen 2026-11-12) | treg `companies.jobs.search`; count posts per company in Clay | Titles: dispatch rider, delivery rider, driver, warehouse, store attendant, sales representative, merchandiser. Nigeria. 60 days. 3+ posts | 4 | 4 | 3 | 4 | **15** | VERIFIED. Maps to Time & Attendance and WhatsApp/USSD (company.md). Threshold of 3 is a starting guess (INFERRED) |
| N2 | Branches in 2+ Nigerian states on Google Maps; timing version: a new branch opened since the last run | Physical-world (location count) | S2, S3 | Google Maps listings for the brand name | treg `google.serp.maps` (crawl4ai $0.00023) | Brand name match; distinct states; monthly delta for new branches | 3 | 3 | 4 | 4 | **14** | VERIFIED (static count is COMPOUND; the new-branch delta is the timing signal). Multi-state remittance is a product feature (signals.md, A2) |
| N3 | Finance job posts that name an ERP (SAP, Oracle, Microsoft Dynamics, Sage) | Companies using specific software (from job text) | S3 | Job descriptions from U1-type searches on finance titles | Same as U1, description filter | Titles: Financial Controller, Finance Manager, Accountant. Keywords: SAP, Oracle, Dynamics, Sage | 3 | 3 | 3 | 3 | **12** | VERIFIED (COMPOUND). BuiltWith cannot see back-office ERPs (framework), so job text is the only route. "Your ERP stays" angle (voice.md) |
| N4 | Firm advertises payroll administration or outsourcing for clients | Website content mentions | S4 | Clutch (clutch.co/ng/accounting/payroll), ConnectCiti, Finelib directories; firm websites | treg Google SERP or `crawl4ai` skill to read the services page | "payroll outsourcing", "payroll administration", "payroll processing", "employer of record"; Nigeria | 4 | 2 | 4 | 4 | **14** | VERIFIED. Static: pair with U1 (hiring payroll officers) for timing |
| N5 | Named in news as a PenCom, ICPC or recovery-agent pension defaulter | Regulatory | S2, S3 | Nairametrics and other news (market.md, section 4) | treg news or SERP search | Company names in PenCom recovery stories | 5 | 1 | 2 | 1 | **9** | KILLED. Stories name sectors, rarely companies, and opening with "you were named a defaulter" breaks the no-diagnosis guardrail. Use PenCom enforcement as a content theme instead |
| N6 | Former Bento Africa customer | Competitor customers | S1, S2 | No list exists (signals.md, A2) | None | | 5 | 1 | 1 | 3 | **10** | KILLED. Not programmatically findable. Bento stopped in Feb 2025, so these firms have most likely moved already (INFERRED) |
| N7 | Bids for government contracts (needs a Pension Clearance Certificate) | Public procurement | S3 | Nigerian procurement portals | Not checked | | 4 | 2 | 1 | 3 | **10** | HOLD. The PCC-for-contracts rule is unverified (market.md, section 4; signals.md, Gaps) |
| N8 | LSETF free-access cohort reaching the end of 12 months | Niche play | S1, S2 | Participants not named (customers.md) | None | | 4 | 1 | 1 | 4 | **10** | KILLED. No data. Ask PaidHR for its own cohort list instead (they are existing users) |
| N9 | 31 Jan 2027 annual PAYE return | Regulatory | All | Fixed date (signals.md, A1) | None needed | | 3 | 5 | 5 | 4 | n/a | KILLED as a signal: it applies to every Nigerian employer, so it does not separate accounts. Use it as a **timing angle** inside U1, U3 and N1 campaigns until January |
| N10 | Medium-taxpayer e-invoicing enforcement, Jan to Mar 2027 | Regulatory | S3 | market.md, section 3 | None | | 1 | 3 | 1 | 1 | **6** | KILLED. Not a payroll rule (INFERRED link only) |
| N11 | Minimum wage change | Regulatory | All | market.md, section 6 | None | | 4 | 5 | 5 | 4 | n/a | HOLD until a new figure and date are agreed. Then it becomes a timing angle like N9 |
| N12 | Staff complaining publicly about wrong deductions or late salaries (X, Nairaland, Glassdoor) | Content / complaints | S2, S3 | Social posts | `lead-signals` skill | | 5 | 2 | 2 | 1 | **10** | HOLD. Hard to tie posts to employers, and the story is a diagnosis. Worth one small `lead-signals` test |

### The compounding view
- **8 universal signals, 4 live niche signals.** U1 is one build that serves all four segments: only the title keywords change (HR and payroll titles for S1 to S3, payroll officers at client-service firms for S4). U2, U3 and U6 reuse the same job and people searches with different filters.
- **Niche builds:** N1 (frontline hiring) and N2 (Google Maps) for S2, N3 (ERP in job text) for S3, N4 (website services) for S4. S1 needs no niche build: funding (U5) is its extra layer.
- **What changes between segments** is the angle (Matrix 1) and the lead product, not the build.

### Signals by segment (primary cells)

| Type | Signal | S1 Funded tech/fintech | S2 Frontline operators | S3 Large employers | S4 Payroll partners |
|---|---|---|---|---|---|
| Universal | U1 HR/payroll hiring | Play: first or new HR/payroll hire. Product: payroll + remittance | Play: HR/payroll hire at a logistics, retail or manufacturing firm. Product: T&A + payroll | Play: payroll or HR ops hire at 200+. Product: payroll + ERP journals | Play: hiring payroll officers to serve clients. Product: white-label |
| Universal | U2 Manual payroll skills in the post | Stack on U1 | Stack on U1 | Stack on U1 | |
| Universal | U3 New HR/finance leader | Product: payroll + remittance | Product: T&A + payroll | Product: full suite; CFO angle | |
| Universal | U4 Headcount growth | Product: payroll + HR | | Weak | |
| Universal | U5 Funding | Product: payroll + remittance | Killed | Killed | Killed |
| Universal | U6 Hiring in a second country | Product: cross-border payroll | | Product: cross-border payroll | |
| Universal | U7 Tax-reform content engagement | Routing layer | Routing layer | Routing layer | |
| Universal | U8 Lookalike | Seeds: Mono, Risevest, Kredete, GetEquity, GO54 | Seeds: Heroshe, ReelFruit | | |
| Niche | N1 Frontline hiring velocity | | Product: T&A + WhatsApp/USSD + EWA | | |
| Niche | N2 Multi-state branches / new branch | | Product: multi-state remittance | Product: multi-state remittance | |
| Niche | N3 ERP named in finance posts | | | Product: "Your ERP stays" | |
| Niche | N4 Offers payroll services | | | | Product: white-label / Payroll API |

---

## Build first: top five

| Rank | Signal | Segments | Why first |
|---|---|---|---|
| 1 | **U1 HR/payroll hiring, with U2 stacked** | S1 and S2, same volume each | Highest score (16). One build serves every segment. Running it in S1 and S2 at equal volume is the cleanest test of the open industry question in icp.md (H1 vs H2). The job post is a natural opener, and U2 picks out firms whose new hire will be doing payroll by hand |
| 2 | **N1 Frontline hiring velocity** | S2 | The only signal built for H2. It points straight at the product's 2026 direction (geo-tracked attendance, WhatsApp/USSD, EWA). Rider and store-staff posts were easy to find on Jobberman. Pair with N2 to separate single-site firms from multi-state ones |
| 3 | **U6 Hiring in a second African country** | S1, S3 | Clearest product link: cross-border payroll, and one of the few things local payroll-only rivals like Salario and NotchHR are not described as doing (INFERRED from competitors.md). Lower volume, so run it as a small always-on list |
| 4 | **N4 Offers payroll services, timed by U1** | S4 | One signed partner runs many client payrolls (INFERRED), and PaidHR already sells to this buyer. The list is cheap to build from directories plus `crawl4ai`. Needs partner pricing and terms from PaidHR first |
| 5 | **U3 New HR or finance leader (90 days)** | S1, S2, S3 | Cheap (Apollo people search is $0 in treg) and reuses the people search the other workflows need anyway. Gives a second reason to reach the same accounts and a different persona (finance) |

**Not in the top five, and why:** U5 funding scores well on story but volume is low (six major Nigerian deals in April 2026, Nairametrics). Add it as a secondary enrichment on S1 instead of its own workflow. N9 (the 31 Jan return) is the timing angle for all five until January, not a workflow.

**Before any build:** run one small test pull for U1 and N1 (Nigeria, last 30 days) to check that the job providers actually cover Nigerian boards. If they don't, U1, U2, U6, N1 and N3 all drop in the Data score. That is the single biggest risk in this matrix.

---

## Matrix 3: Workflow tracker

All rows are Planned. Columns after Mother Intent are filled as campaigns launch (see the framework's Matrix 3 template for the full column list).

| W # | Status | Workflow name | Mother intent | Segment(s) | Persona | Campaigns | Launch date | Channel | Clay URL | Copy URL | Total leads | Bounce |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| W1 | Planned | HR and payroll hiring (+ manual payroll skills) | Hiring specific roles | S1, S2 | | | | | | | | |
| W2 | Planned | Frontline hiring velocity | Hiring velocity | S2 | | | | | | | | |
| W3 | Planned | Hiring in a second African country | Hiring offshore | S1, S3 | | | | | | | | |
| W4 | Planned | Payroll service firms hiring payroll staff | Website content mentions | S4 | | | | | | | | |
| W5 | Planned | New HR or finance leader | New leadership | S1, S2, S3 | | | | | | | | |
| W6 | Planned | Multi-state branches / new branch | Physical-world | S2, S3 | | | | | | | | |
| W7 | Planned | ERP named in finance job posts | Specific software (job text) | S3 | | | | | | | | |
| W8 | Planned | Headcount growth | Hypergrowth | S1 | | | | | | | | |
| W9 | Planned | Recently funded | Fundraising | S1 | | | | | | | | |

U7 (content engagement) and U8 (lookalikes) are routing and enrichment layers, not standalone workflows.

---

## Open questions for PaidHR
- Customer split by industry and headcount (from icp.md): decides whether S1 or S2 leads.
- Partner pricing and terms for white-label (S4).
- Approval to name customers in copy (facts.md, all Pending).
- The LSETF cohort list and any lost-deal or churn list: both are warm signals PaidHR holds and we do not.
- Who signs at 200+ companies: CEO or CFO (personas.md).

## Sources added in this pass (checked 2026-11-12)
- S1 examples: [Seedtable, best fintech startups in Lagos](https://seedtable.com/best-fintech-startups-in-lagos); [Seedtable, Lagos startups 2026](https://seedtable.com/companies/city/nigeria/lagos); [Nairametrics, six deals in April 2026](https://nairametrics.com/2026/05/10/nigerian-startups-attract-4-million-across-six-deals-in-april-2026/); [Nairametrics, Nectarfi pre-seed](https://nairametrics.com/2026/04/15/nectarfi-raises-170000-pre-seed-to-tackle-crypto-fragmentation/)
- S2 examples: [Jobberman, JAM-Forte Logistics](https://www.jobberman.com/listings/dispatch-rider-n99g69); [Jobberman, TL First Group](https://www.jobberman.com/listings/delivery-rider-m0p96q); [Jobzilla, Fleximotion](https://www.jobzilla.ng/jobs/dispatch-riders-at-fleximotion-268101); [HotNigerianJobs, 3F Logistics](https://www.hotnigerianjobs.com/print/977245/); [HotNigerianJobs, Telcapit](https://www.hotnigerianjobs.com/print/943797/); [Nairametrics, supermarket chains by store count](https://nairametrics.com/2025/03/30/top-10-supermarket-chains-in-nigeria-by-store-count-dominating-the-retail-sector)
- S3 examples: [BusinessDay, Marketsquare](https://businessday.ng/news/article/marketsquare-expands-presence-in-lagos-with-launch-of-additional-two-stores/); [SPAR International, SPAR Nigeria](https://spar-international.com/news/spar-nigeria-grows-its-retail-footprint-with-two-new-stores)
- S4 examples: [Clutch, payroll services in Nigeria](https://clutch.co/ng/accounting/payroll); [ConnectCiti, Phillips Outsourcing](https://connectciti.com/listing/phillips-outsourcing-limited); [ConnectCiti, HumanManager](https://connectciti.com/listing/humanmanager-limited); [Outsource Accelerator, Rovedana](https://www.outsourceaccelerator.com/company/rovedana-limited); [SOL Nigeria](https://solnigeria.com/); [BDO Nigeria payroll services](https://ng-www.bdo.global/en-gb/services/tax/payroll-services); [Finelib, payroll services](https://www.finelib.com/business/finance/payroll-services)
- Detection tools and prices: treg `catalog_search` results for job postings, funding rounds, Google Maps search, job-change detection, LinkedIn post search and headcount history.
