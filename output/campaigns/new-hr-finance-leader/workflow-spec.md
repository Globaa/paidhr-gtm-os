# Workflow Spec: New HR or finance leader

**Status:** COPY-APPROVED 2026-11-18 · facts approved and sender named 2026-11-18 · Apollo test pull skipped (decision 2026-11-18) · structure revised 2026-11-18 (13 fixes from the Clay build guide, approved by Fredrick; copy unchanged) · not sendable until `/campaign-qa` passes

Campaign 4 in `output/campaigns/campaign-strategy.md`. Signal U3 in `context/icp/signal-matrix.md` (workflow W5). Pipeline steps from `context/docs/gtm-system.md`. Built with `/build-workflow-spec`; structure approved by Fredrick on 2026-11-18 at four stops (mother intent and inclusion, sources and signals table, angle matrix, variables and cadence).

**Facts approved and sender named** (Fred, Product Growth Lead) on 2026-11-18 in `context/messaging/facts.md`. **Sending is still blocked** until `/campaign-qa` passes.

**Revision 2026-11-18.** Thirteen fixes found while writing `new-hr-finance-leader-clay-build-guide.md`, applied with four decisions by Fredrick: S1 drops "funded"; a tie-break for two same-persona leaders at one company; Accounting excluded at S3; "still employed" is a required gate. The other nine document how the build really works in Clay (previous employer needs Enrich Person, 90 days read as 3 months, keyword industry match, size floor, HubSpot exclusions, extra HR titles, PAYE season evaluated at push time, `e2body`, company-name cleaning). No copy line changed.

---

## 1. Mother intent [ENGINE]

An HR leader or finance leader started a new job at an ICP company in Nigeria in the last 90 days.

- Catalog intent: **New leadership (past 90/180 days)**, Leadership & People Movement (`knowledge-base/playbooks/workflow-anatomy.md`). Signal U3, scored 13/20 (signal-matrix.md).
- A new job, not a promotion: the previous employer must be a different company. Internal promotion is a separate catalog intent ("New target persona promotion").
- Risk: the buying reason is INFERRED ("a new HR leader typically re-evaluates tools in their first 90 days", signals.md A3). No customer evidence. The angles do not assume a tool switch; they speak to the first filing cycle under the new leader.

## 2. Inclusion / exclusion [ICP]

Nigeria only. Kenya and Ghana are not built (campaign 1 holds them for lack of approved local filing facts).

### Include

| Filter | Value | Source |
|---|---|---|
| Geography | HQ or main operation in Nigeria | icp.md; signal-matrix.md |
| Segments | **S1** tech and fintech, 20 to 199 staff (industry contains software, financial services, IT services or internet). Funding is not read (decision 2026-11-18). **S2** frontline operators, 20 to 199 staff (industry contains transport, logistics, retail, manufacturing, food and beverage or warehousing). **S3** large employers, 200+ staff, any industry except Accounting, **finance leaders only** (decisions 2026-11-18). Industries are matched by keyword because LinkedIn labels vary ("Software Development", "Technology, Information and Internet", "Truck Transportation"). Clay's size buckets start at 11-50, so the floor of 20 is checked after company enrichment, from HQ headcount | signal-matrix.md S1 to S3; campaign-strategy.md idea 4 |
| HR leader titles | HR Manager, Head of HR, Head of Human Resources, Head of People, HR Director, HR Lead, Human Resources Manager, Human Resources Director, People Operations Lead/Manager, Head of People & Culture, Chief People Officer, CHRO (CPO, CHRO and Head of Human Resources added 2026-11-18) | personas.md (champion); U3 filters |
| Finance leader titles | CFO, Head of Finance, Finance Director, Finance Manager, Financial Controller | personas.md (economic buyer at 100+, influencer); U3 filters |
| Start date | Current role started 3 months or less before the pull month. Start dates come as year and month (`YYYY-MM`), so this can include starts up to about 120 days back; accepted 2026-11-18. A missing start date fails the trigger | U3 |
| Recipient | The new leader. If a new HR leader and a new finance leader are both found at one company, contact both (max 2 per company, starting rule, gtm-system.md step 7). **Tie-break** (decision 2026-11-18): if two leaders of the same persona qualify at one company, contact the most senior (Chief, Head or Director titles beat Manager or Lead); if still tied, the one who started earlier. The other row is held, not deleted | |

### Exclude

| Who | Why |
|---|---|
| Internal promotions (previous employer is the same company) | Different catalog intent |
| Junior titles: HR Officer, HR Assistant, Payroll Officer, Accountant, Analyst, Intern | Not leaders. Payroll officers are covered by campaign 1 |
| HR leaders at S3 (200+) companies | Decision 2026-11-18: S3 is finance-only. No named 200+ proof (signal-matrix.md S3) |
| Previous employer is a PaidHR customer | Champion migration, a separate intent ("don't combine with other intents", workflow-anatomy.md) |
| Previous employer is a competitor (SeamlessHR, Workpay, NotchHR, Eazipay, Bujeti, Salario) or PaidHR | competitors.md |
| PaidHR customers, open deals, contacts in an active sequence (HubSpot) | gtm-system.md steps 3 and 8. HubSpot is not connected yet: until it is, PaidHR exports these lists and they are added to the Exclusions CSV (`exclusions.csv`, matched on domain) |
| Unsubscribed or do-not-contact; contacted in the last 90 days | gtm-system.md step 8 (90 days is a starting rule, not agreed) |
| Companies in an active campaign 1 sequence | One active sequence per company |
| Firms that sell payroll or staffing services | S4 partners, a separate play. Read: industry Staffing and Recruiting or Human Resources Services (any segment), or Accounting at S3 (decision 2026-11-18) |
| Under 20 staff; 20 to 199 staff outside S1 and S2 industries | Outside the segments |
| LSETF free-access cohort | Existing users; list needed from PaidHR |
| Not confirmed still in the role | Signal 8 must be YES (decision 2026-11-18) |

## 3. Sources [ENGINE]

| Purpose | Source | Tool and price |
|---|---|---|
| Trigger: new leader and start date | LinkedIn profile data via Apollo (current title, current-role start date, location) | treg `apollo.people.search` ($0) or Clay "Find People". **Coverage of Nigerian profiles and freshness of start dates unknown; test pull skipped (2026-11-18), watch the first live pull** |
| Previous employer | Employment history (list of past jobs) | In Clay: Enrich Person, about 1 Action per person (the free people search doesn't return job history). Via treg Apollo: the same record, $0 |
| Job-change check when history is missing or unclear | LinkedIn job-change detection | treg `leadmagic.x.job-change-detector` ($0.075), only on rows the job history cannot settle. Runs in treg, outside Clay |
| Still employed (signal 8) | Enrich Person job history | Clay Use AI, about 1 Action plus AI cost per person |
| Domain, headcount, industry, HQ country | Apollo company data | Clay, own key. Credit cost unknown until `/calculate-costs` |
| Other HR staff at the company | LinkedIn and Apollo people data | treg `apollo.people.search` ($0) or Clay "Find People" |
| Previous-employer exclusions | HubSpot customer list; competitor list from competitors.md | HubSpot; static list in Clay |
| Email | Clay waterfall (own keys first) and verification | Clay |
| Exclusions | HubSpot company check, then contact check | HubSpot |

## 4. Signals table [ENGINE]

| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | New HR or finance leader, in role 90 days or less | **trigger** | Regex on current title. HR: `Head of (HR\|People\|Human Resources)\|HR (Manager\|Director\|Lead)\|Human Resources (Manager\|Director)\|People (Operations\|& Culture) (Lead\|Manager)\|Chief People Officer\|CHRO`. FIN: `CFO\|Chief Financial Officer\|Head of Finance\|Finance (Director\|Manager)\|Financial Controller`. Negative regex: `Officer\|Assistant\|Intern\|Analyst\|Accountant` (except "Chief Financial Officer" and "Chief People Officer"). Current-role start month 3 months or less before the pull month. Company HQ Nigeria | boolean; `job_title`, `start_date` |
| 2 | Joined from another company | **trigger** (1 AND 2) | Previous employer's domain is not the current company's domain. Unclear rows go to the job-change detector; still unclear means dropped | boolean; `previous_company` |
| 3 | Persona | **router** | Which regex in signal 1 matched | `HR` / `FIN` |
| 4 | Segment | **router** | Formula on enriched HQ country, headcount and industry: HQ not Nigeria → excluded; 200+ AND industry not Accounting → `S3`; 20 to 199 AND S1 keyword → `S1`; 20 to 199 AND S2 keyword → `S2`. `S3` AND `HR` → excluded. Else excluded | `S1` / `S2` / `S3` |
| 5 | Other HR staff at the company | **router**, HR leaders only | Count of current employees with titles matching `HR\|Human Resources\|People\|Payroll\|Talent`, **excluding the new leader**. Clusters: **0** = `first`, the first HR leader; payroll and remittance sat with the founder, MD or finance until now. **1 or more** = `team`, joined an existing HR team with a process already in place. Not read for finance leaders. Raw count never goes to copy | `first` / `team` |
| 6 | Previous company | **context-clause** | Signal 2 output, cleaned (no "Ltd", "Limited", "Nigeria"). Never routes. Acknowledges the move only; never a tool comparison | `previous_company` |
| 7 | Annual PAYE return season | **context-clause** | Date on or before 2027-01-15, evaluated when the copy is generated (push time), not when each email sends. **Last push with season copy: 2027-01-08**, so Email 2 (day 3 to 4) lands before 15 January | boolean. Switches the 31 January return anchor on, then off |
| 8 | Still employed | **trigger** (1 AND 2 AND 8) | AI classification on the Enrich Person job history: is the person still at this company in this or a similar title? Hard enum `YES` / `NO` / `REVIEW`; only `YES` passes, `REVIEW` goes to manual review (decision 2026-11-18) | `YES` / `NO` / `REVIEW` |

**Not routers, on purpose:** funding (U5) is not needed by this brief; days in role does not change the story (both 10 and 80 days land before 31 January); the previous employer's industry or size would need an eyeball read and does not change the angle.

## 5. Routing matrix with angles [ENGINE]

Persona x segment x HR staff = **7 cells, all LIVE (NG).** HR: 2 segments x 2 HR-staff clusters = 4 cells. Finance: 3 segments = 3 cells. S3 + HR is excluded.

**Timing rule (all cells):** until 2027-01-15 the angle anchors on the 31 January annual PAYE return, the first under the 2026 bands, which lands inside the new leader's first months. After that date the anchor becomes "the first filing cycles you own". Every angle survives without the date.

| Cell | Deterministic read | What it tells us (inferred situation) | The angle it earns | Status |
|---|---|---|---|---|
| H1 | HR + S1 + first | A tech company hired its first HR leader in the last 90 days. The founder or finance ran payroll and filed PAYE and pension by hand until now; the new leader is taking it over, and the 31 January return falls in their first months. Contact: new HR leader | **Take payroll over as a system, not someone else's routine.** Before the first annual return, set payroll up so PAYE, pension, NHF and NSITF file after each run with a receipt for each, and the handover from the founder is a setup, not a monthly checklist. Proof: FAMASI (founder ran payroll by hand before) | LIVE |
| H2 | HR + S1 + team | A new HR leader joined a tech company with an HR team. They inherit a payroll and remittance process built before them, and the first annual return they sign off is on the new 2026 bands. Contact: new HR leader | **Know the process you inherited before your first return.** Remittance filed each run with a receipt for each gives them evidence to check, not a process to trust. Engine already on the 2026 bands. Never imply the current process is wrong. Proof: Heroshe (receipts as "evidence for the payment") | LIVE |
| H3 | HR + S2 + first | A logistics, retail or manufacturing company hired its first HR leader. The MD or accountant paid staff until now; many staff likely work on sites or shifts (INFERRED from industry). The new leader is building HR from zero and inherits pay for that workforce. Contact: new HR leader | **Start HR with pay for shift and site staff already running.** Hours from attendance flow into pay, staff check payslips on WhatsApp or USSD with no app, remittance files after each run, so the first months go on building HR, not catching up on payroll. Proof: Heroshe (logistics) | LIVE |
| H4 | HR + S2 + team | A new HR leader joined a frontline company with an HR team. Payslip, leave and pay questions from staff who don't sit at desks come to HR (INFERRED from industry). Contact: new HR leader | **Staff who don't sit at desks serve themselves from the start of your tenure.** Payslips and leave on WhatsApp or USSD, remittance with a receipt for each, which takes routine requests and agency filing off the team the new leader now runs. Proof: Heroshe (logistics; payslip requests and remittance) | LIVE |
| F1 | FIN + S1 | A new finance leader at a tech company now funds and reconciles payroll. The first annual PAYE return on the 2026 bands closes on their watch. Contact: new finance leader | **Your first annual return reconciles without rework.** Deductions on the 2026 bands every run, remittance filed per run with receipts, so January ties back to the monthly runs. Proof: 2025 filing record (₦934M PAYE, ₦754M pension, on time). No named finance customer exists | LIVE |
| F2 | FIN + S2 | A new finance leader at a frontline company pays many hourly or shift staff, likely across several sites (INFERRED). Pay depends on hours someone collects. Contact: new finance leader | **Payroll that starts from recorded hours.** Hours from attendance flow into pay, deductions on the 2026 bands, remittance after each run: finance reconciles one record, not site sheets. Proof: 2025 filing record. Heroshe as a logistics peer, but its proof is remittance, not finance | LIVE |
| F3 | FIN + S3 | A new CFO or controller at a 200+ employer likely runs an ERP or finance system (INFERRED). They will review the systems feeding it, payroll included, and the first annual return on the new bands is theirs. Contact: new finance leader | **Your ERP stays; payroll feeds it.** Payroll on the 2026 bands, journals pushed into the finance system, remittance filed per run: nothing to replace, one source that posts clean journals. Never name an ERP (no named integration exists, company.md). Proof: 2025 filing record only; no enterprise case study | LIVE |

**Branch coverage.** Every cell has `{{job_title}}`, `{{company_name}}` and `{{previous_company}}` (signals 1, 2 and 4 are required to enter). Only H cells read HR staff: "first HR leader" appears only in H1 and H3, never in finance cells. No cell needs a job post, funding or a raw count. After 2027-01-15 every angle holds on "first filing cycles".

**Niche-native test.** Every angle is tied to one moment: a person new in the seat, inheriting or setting up payroll just before the first annual return on the new bands. The H1/H2 split (handover versus inherited process) and the F3 ERP line could not be sent unchanged by any HR vendor's generic new-leader email.

**Facts the angles depend on (approved in facts.md 2026-11-18):** Variant B/C; the tax-law line; the 31 January return; the 2025 filing record; FAMASI and Heroshe; WhatsApp and USSD access; attendance into payroll; receipts per remittance (added 2026-11-18); ERP journals (added 2026-11-18, F3 only).

## 6. Variables contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{first_name}}` | People table: first name | All |
| `{{company_name}}` | Companies table: company name, cleaned (no "Ltd", "Limited", "PLC" or "Nigeria" at the end) | All |
| `{{job_title}}` | Signal 1: current title, cleaned (no department codes, locations or "& Admin" tails) | All |
| `{{previous_company}}` | Signal 6: previous employer, cleaned | All |

**Sequencer field:** Email 2 L1 and L2 are joined into one field, `e2body` (a blank line and L2 only when L2 exists), so F1 and F2 rows don't show an empty line.

**Routing fields, never shown in copy:** `persona`, `segment`, `hr_team` (H cells only), `paye_season`, `still_employed`, `start_date` (QA and tie-break only), `linkedin_url` (QA only).

**Not allowed in copy:** raw HR headcount, start date or days in role, the previous company's tools or any comparison with them, funding, anything not in this table. Sender name and signature come from facts.md (Fred, Product Growth Lead).

## 7. Cadence and channel [ENGINE]

- **Periodicity:** always-on. Weekly people pull for starts in the last 90 days, deduped at person level; each person enters once. If the company is in an active campaign 1 sequence, the person waits until it ends.
- **Channel:** email plus LinkedIn, same mechanics as campaign 1 (gtm-system.md step 9).
  - Email in PlusVibe: step 1 day 0, step 2 day 3 to 4, step 3 day 7 to 10.
  - LinkedIn by a rep from HubSpot tasks: connection request with no note 1 to 2 days after email 1; DM 1 after acceptance; DM 2 three to five days later.
  - No valid email: LinkedIn-only track. Expect a larger LinkedIn-only share than campaign 1 (new hires' emails are often not in databases yet, INFERRED).
  - A reply on either channel pauses the other (n8n).
- **Timing line:** the 31 January 2027 return anchor runs until 2027-01-15, then every cell switches to "first filing cycles". Last push with season copy is 2027-01-08; pushes after that re-run the copy so new leads get the fallback lines.
- **Order of paid steps:** Enrich Company, then Enrich Person, then the still-employed check, then the email lookup, each only on rows that passed the step before.
- **Before launch:**
  - Test pull of Apollo start dates: **skipped** (Fredrick, 2026-11-18). Risk accepted: if start dates for Nigerian profiles lag 60+ days, most of the 90-day window is gone. Watch the start-date spread in the first live pull.
  - facts.md approvals and a named sender: done 2026-11-18.
  - `/campaign-qa` passes on the Clay table and the PlusVibe campaign.

---

## Copy

Written with `/campaign-copy` and reviewed as the target personas (new Head of People at a tech company, new HR Manager at a logistics firm, new CFO at a 200+ employer) plus the human test on 2026-11-18. Channel: email (3 steps) plus LinkedIn (empty connection request, 2 DMs). Every claim is in `context/messaging/facts.md`, approved 2026-11-18. Sender: Fred, Product Growth Lead (`{{sender_signature}}` = "Fred" / "Product Growth Lead, PaidHR"; `{{sender_name}}` = "Fred"). Routes are exhaustive (every live lead is H1 to H4 or F1 to F3), so lines routed by cell have no fallback; lines routed by `paye_season` do.

### Email 1 (day 0, new thread)

**Subject (static):** `{payroll in your first months|the first filings you own|your first payroll filings}`

**Greeting:** `{{first_name}},`

**L1 (dynamic, by `paye_season`):**
- + Send date on or before 2027-01-15: You joined {{company_name}} from {{previous_company}} a few months before the first annual PAYE return on the 2026 tax bands, due by 31 January.
- FALLBACK (after 2027-01-15): You joined {{company_name}} from {{previous_company}} recently, so the next few payroll filings will be the first ones you own.

**L2 (dynamic, by cell):**
- + H1: I couldn't find anyone else in HR there on LinkedIn, so my guess is payroll is coming over to you from the founder or finance.
- + H2: {{company_name}} already has an HR team, so I imagine you're picking up a payroll process set up before you joined.
- + H3: From what I can find on LinkedIn, you're the first HR person there, so pay for shift and site staff probably lands with you too.
- + H4: With an HR team already there, I'd guess a fair share of what reaches you is payslip and leave questions from staff who aren't at a desk.
- + F1: I'd guess getting payroll on the new bands to reconcile each month now sits with you.
- + F2: I'd guess a lot of {{company_name}}'s staff work shifts or on sites, so pay starts from hours collected at each location.
- + F3: At {{company_name}}'s size, payroll is probably one of several systems feeding your ERP, and one you'll look at early.

**L3, value prop (static, facts.md Variant C):** PaidHR runs payroll and files PAYE, pension, NHF and NSITF on time every run.

**L3b (dynamic, by cell):**
- + H1: Each remittance comes with a receipt, so you can see every filing went through.
- + H2: Each remittance comes with a receipt you can check yourself.
- + H3: Hours from attendance flow into payroll, and staff check payslips on WhatsApp and USSD.
- + H4: Employees can check payslips and request leave on WhatsApp and USSD.
- + F1: It's already updated for the new tax law, with a receipt for each remittance.
- + F2: Hours from attendance flow straight into payroll, and it's already updated for the new tax law.
- + F3: It also pushes payroll journals into the enterprise systems your finance team lives in.

**Social proof (dynamic, by cell):**
- + H1: At FAMASI, a health tech startup, the CEO got back about an hour on every payroll run.
- + H2: Heroshe's statutory remittances used to take two weeks or more before they moved to PaidHR.
- + H3, H4: At Heroshe, a logistics company, statutory remittances used to take two weeks or more before they moved to PaidHR.
- + F1, F2, F3: ₦934M in PAYE and ₦754M in pension were filed through PaidHR in 2025, all on time.

**CTA (dynamic, by persona and HR staff):**
- + First HR leader (H1, H3): Would it help to have this set up before your next payroll?
- + HR leader joining a team (H2, H4): Is payroll something you plan to look at soon?
- + Finance (F1, F2, F3): Worth comparing notes on the payroll side?

**Sign-off:** `{{sender_signature}}`

### Email 2 (day 3 to 4, same thread, subject "Re:")

**Greeting:** `{{first_name}},`

**L1 (dynamic, by persona):**
- + HR (H1 to H4): ₦934M in PAYE and ₦754M in pension were filed through PaidHR in 2025, all on time.
- + Finance (F1 to F3): PaidHR customers have seen payroll processing cut from over a week to one day.

**L2 (dynamic, by persona and `paye_season`):**
- + HR, send date on or before 2027-01-15: It's already updated for the new tax law, ahead of the 31 January return.
- + HR, after 2027-01-15: It's already updated for the new tax law.
- + F3: It's already updated for the new tax law.
- F1, F2: no line (tax-law line already in Email 1).

**CTA (dynamic, by persona):**
- + HR: Want me to show you what the remittance receipts look like?
- + Finance: Would it help to see a payroll run from the finance side?

**Sign-off:** `{{sender_signature}}`

### Email 3 (day 7 to 10, same thread, subject "Re:")

**Greeting:** `{{first_name}},`

**CTA, right person (dynamic, by persona and HR staff):**
- + First HR leader (H1, H3): If payroll is still with finance at {{company_name}} for now, who would be best to speak with?
- + HR leader joining a team (H2, H4): Does payroll sit with you at {{company_name}}, or with someone else on the team?
- + Finance (F1, F2, F3): If payroll is run by HR at {{company_name}}, should I speak with them instead?

**Sign-off:** `{{sender_signature}}`

### LinkedIn (rep sends from HubSpot tasks)

**Connection request:** empty, no note, 1 to 2 days after Email 1.

**DM 1 (1 to 2 days after acceptance, dynamic by persona and HR staff):**
- + First HR leader (H1, H3): {{first_name}}, is payroll moving over to you at {{company_name}}, or staying where it is for now?
- + HR leader joining a team (H2, H4): {{first_name}}, is payroll part of what you're taking on as {{job_title}}?
- + Finance (F1, F2, F3): {{first_name}}, is payroll one of the things you're reviewing in your first few months as {{job_title}}?

**DM 2 (3 to 5 days after DM 1, static):** `Asking because PaidHR runs payroll and files PAYE, pension, NHF and NSITF on time every run, with a receipt for each. {Happy to show you if useful.|Glad to walk you through it if it helps.}` Sign: `{{sender_name}}`

### Sample: H3 (new HR Manager, logistics, no other HR staff), Email 1 as received

> **Subject:** payroll in your first months
>
> {{first_name}},
>
> You joined {{company_name}} from {{previous_company}} a few months before the first annual PAYE return on the 2026 tax bands, due by 31 January.
>
> From what I can find on LinkedIn, you're the first HR person there, so pay for shift and site staff probably lands with you too.
>
> PaidHR runs payroll and files PAYE, pension, NHF and NSITF on time every run.
>
> Hours from attendance flow into payroll, and staff check payslips on WhatsApp and USSD.
>
> At Heroshe, a logistics company, statutory remittances used to take two weeks or more before they moved to PaidHR.
>
> Would it help to have this set up before your next payroll?
>
> {{sender_signature}}

### Sample: F3 (new CFO, 200+ employer), Email 2 as received

> **Subject:** Re: the first filings you own
>
> {{first_name}},
>
> PaidHR customers have seen payroll processing cut from over a week to one day.
>
> It's already updated for the new tax law.
>
> Would it help to see a payroll run from the finance side?
>
> {{sender_signature}}

### Copy notes

- Word counts (E1, season L1): H1 98, H2 93, H3 98, H4 98, F1 80, F2 87, F3 85. Under the 120 ceiling; the 60 target is not reachable without dropping a line. E2 about 25 to 40. E3 about 15 to 18. DM 1 about 13 to 18. DM 2 about 26.
- Review changes (2026-11-18): rewrote L1 (both variants) so it no longer reads mechanically; removed repeated `{{company_name}}` in L2 (H1, H4) and cut H2 and H3; smoothed F1 and F2; made the H1 receipts line concrete; removed spintax from every dynamic CTA (E1, E2), since dynamic lines resolve to one sentence. Campaign 1 had the same issue; fixed there on 2026-11-18.
- `{{job_title}}` is used only in DM 1; Email 1 no longer uses it.
- Wording approved with the facts on 2026-11-18: "PaidHR customers have seen payroll processing cut from over a week to one day" (the source is an Accion aggregate, no named customer); "with a receipt for each" / "Each remittance comes with a receipt" (receipts fact added 2026-11-18); FAMASI and Heroshe industry descriptors.
- Before sending: the 31 January date is checked against the tax authority and `/campaign-qa` passes. Facts and sender done 2026-11-18; Apollo test pull skipped. Sequencer export (`sequencer-export.md`) not yet written.
