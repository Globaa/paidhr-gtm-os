# Workflow Spec: HR or payroll hire, manual payroll skills

**Status:** STRUCTURE-APPROVED 2026-11-17 · copy pending · NG cells live, KE and GH cells on HOLD

Campaign 1 in `output/campaigns/campaign-strategy.md`. Signals U1 and U2 in `context/icp/signal-matrix.md` (workflow W1). Pipeline steps from `context/docs/gtm-system.md`. Built with `/build-workflow-spec`; structure approved by Fredrick on 2026-11-17 at two stops (signals table, angle matrix).

**Sending is blocked** until the facts this spec uses are approved in `context/messaging/facts.md` (all Pending) and a sender is named. Copy can be drafted with `{{placeholders}}`.

---

## 1. Mother intent [ENGINE]

A company with 20 to 199 staff in Nigeria, Kenya or Ghana, in tech/fintech (S1) or a frontline industry (S2), has posted in the last 30 days for an HR or payroll role whose job description includes manual payroll or statutory remittance work.

- Catalog intent: **Hiring specific roles** (`knowledge-base/playbooks/workflow-anatomy.md`).
- Signals 1 and 2 are both required. An HR post without payroll duties is not this workflow.
- Risk: U2 scored 2 of 5 on volume (signal-matrix.md). The first test pull checks whether the AND leaves enough companies.

## 2. Inclusion / exclusion [ICP]

### Include

| Filter | Value | Source |
|---|---|---|
| Geography | HQ or main operation in Nigeria, Kenya or Ghana. Kenya and Ghana cells are HOLD (section 5) | icp.md; decision 2026-11-17 |
| Size | 20 to 199 employees. S2 uses the frontline headcount check (LinkedIn undercounts site staff) | icp.md; gtm-system.md steps 4 and 5 |
| S1 industries | Software, Financial Services, IT Services, Internet | signal-matrix.md S1 |
| S2 industries | Transportation/Logistics, Retail, Manufacturing, Food and Beverage, Warehousing | signal-matrix.md S2 |
| Trigger | Signals 1 and 2 (section 4) | |
| Recipient | Set by signal 4, max 2 contacts per company (starting rule, gtm-system.md step 7) | personas.md |

**Recipient by HR team:**

| HR staff today | Contact 1 | Contact 2 |
|---|---|---|
| 0 | S1: Founder or CEO. S2: Managing Director or owner | Finance Manager or Accountant |
| 1 or more | HR Manager, Head of People or HR lead | S1: Founder or CEO. S2: Managing Director |

### Exclude

| Who | Why |
|---|---|
| PaidHR customers, open deals, contacts in an active sequence (HubSpot) | gtm-system.md steps 3 and 8 |
| Unsubscribed or do-not-contact; contacted in the last 90 days | gtm-system.md step 8 (90 days is a starting rule, not agreed) |
| Recruiter or staffing-agency posts for an unnamed client | No company to target (gtm-system.md step 2) |
| Firms that sell payroll or staffing services | S4 partners, a separate play |
| South Africa HQ | On hold (icp.md) |
| Under 20 or 200+ staff | Outside S1 and S2 |
| Industries outside the S1 and S2 lists | "Unrouted" view, not sent (gtm-system.md step 6) |
| Competitors: SeamlessHR, Workpay, NotchHR, Eazipay, Bujeti, Salario | competitors.md |
| LSETF free-access cohort | Existing users; list needed from PaidHR |
| Companies already in campaign 2 (frontline hiring) or 3 (second-country hiring) | Dedupe across sequences; one active sequence per company |

## 3. Sources [ENGINE]

| Purpose | Source | Tool and price |
|---|---|---|
| Trigger: job posts | NG: LinkedIn Jobs, Indeed, Jobberman. KE, GH: LinkedIn Jobs, Indeed, plus local boards to confirm | treg `companies.jobs.search` (blitzapi $0.001 per call) or Clay "Find Jobs". **Coverage in all three countries unknown: test pull first** |
| Job description | The job record | No extra cost |
| Domain, headcount, industry, HQ country | Apollo company data | Clay, own key. Credit cost unknown until `/calculate-costs` |
| HR staff count; contacts | LinkedIn and Apollo people data | treg `apollo.people.search` ($0) or Clay "Find People" |
| Funding (S1) | Funding database | treg `aviato.companies.funding_rounds` ($0.01) |
| Email | Clay waterfall (own keys first) and verification | Clay |
| Exclusions | HubSpot company check, then contact check | HubSpot |

## 4. Signals table [ENGINE]

| # | Signal | Role | How it's read | Output |
|---|---|---|---|---|
| 1 | HR or payroll job post in the last 30 days | **trigger** | Regex on title: `HR Manager \| HR Officer \| HR Generalist \| Human Resources \| Payroll (Officer\|Accountant\|Specialist\|Admin) \| People (Operations\|Ops)`. Location `Nigeria \| Kenya \| Ghana`. Posted 30 days or less ago | boolean; `job_title`, `job_url` |
| 2 | Manual payroll duties in the description | **trigger** (1 AND 2) | Keyword match by country. **NG:** `PAYE`, `pension remittance`, `NHF`, `NSITF`, `statutory deductions`, `statutory remittance`, `payroll computation`, `Excel` (only with "payroll" in the same sentence). **KE:** `PAYE`, `NSSF`, `SHIF`, `Housing Levy`, `statutory deductions`, `payroll computation`. **GH:** `PAYE`, `SSNIT`, `Tier 2`, `statutory deductions`, `payroll computation`. KE and GH terms are from general knowledge, not the research: confirm before build | boolean; matched-duty list |
| 3 | Segment | **router** | Formula: 20 to 199 AND S1 industry → `S1`; 20 to 199 AND S2 industry → `S2`; else excluded | `S1` / `S2` |
| 3b | Country | **router** | Company HQ country | `NG` / `KE` / `GH` |
| 4 | HR staff at the company | **router** | Count of current employees with titles matching `HR \| Human Resources \| People \| Payroll \| Talent`. Clusters: **0** = no HR, founder/MD or finance runs payroll; **1 or more** = HR exists, adding or replacing. Raw count never goes to copy | `0` / `1+` |
| 5 | Payroll duty to quote | **context-clause** | First match from signal 2 in a fixed order per country (NG: pension remittance, PAYE, statutory remittance, NSITF, NHF, payroll computation) | one duty, in the post's words |
| 6 | Raised money in the last 180 days | **context-clause**, S1 only | Funding round dated in the last 180 days. Never routes | boolean; `round_type` |
| 7 | Annual PAYE return season | **context-clause**, NG only | Send date on or before 2027-01-15 | boolean. Switches the 31 January 2027 return line on, then off |

**Not routers, on purpose:** second-country hiring (U6) and frontline hiring waves (N1) are their own campaigns, so they dedupe rather than route here. Job title does not split the story.

## 5. Routing matrix with angles [ENGINE]

Country (3) x segment (2) x HR staff (2) = **12 cells. 4 live (NG), 8 HOLD (KE, GH).**

**Hold rule:** KE and GH cells keep the NG angle but drop everything Nigeria-only (NHF, NSITF, 2026 tax law, the 31 January return). Named local filings become `{{local_filings}}` until PaidHR confirms what it files in that country and facts.md approves a line. WhatsApp and USSD stay: launched in Nigeria, Ghana and Kenya (company.md). Local proof: none; FAMASI and Heroshe are Nigerian payroll.

**Lifting a hold:** PaidHR confirms local payroll and statutory filing in that country, and facts.md has an approved line for it.

| Cell | Deterministic read | What it tells us | The angle it earns | Status |
|---|---|---|---|---|
| NG-A | NG + S1 + 0 HR | A funded tech company with no HR staff is hiring its first HR or payroll person. The founder or finance person runs payroll and remittance today and is about to hand it to the new hire. Contact: founder or CEO | **Hand payroll over on a system, not a process.** The new hire will take over `{{payroll_duty}}`. If PAYE, pension, NHF and NSITF already file after each run with a receipt for each, the founder hands over a working system, not a monthly routine. Proof: FAMASI. PAYE-season line on | LIVE |
| NG-B | NG + S1 + 1+ HR | A funded tech company with HR is adding a role whose listed duties include `{{payroll_duty}}`. Remittance is still a task someone does each month. Contact: HR lead | **The role you are hiring includes work that can run on its own.** Remittance filed right after each run with receipts as evidence, staff pulling their own payslips, so the new person does HR work. Proof: Heroshe. PAYE-season line on | LIVE |
| NG-C | NG + S2 + 0 HR | A logistics, retail or manufacturing company with no HR staff is hiring its first HR or payroll person. The MD or accountant pays staff and remits today; many staff probably work on sites or shifts (INFERRED from industry). Contact: MD | **Set payroll up for shift and site staff before the hire starts.** Hours from attendance flow into pay, staff check payslips on WhatsApp or USSD with no app, remittance runs after each payroll. The first HR hire inherits `{{payroll_duty}}` already automated. Proof: Heroshe. PAYE-season line on | LIVE |
| NG-D | NG + S2 + 1+ HR | A frontline company with HR is adding a role that includes `{{payroll_duty}}`. HR serves a workforce that mostly does not sit at desks (INFERRED from industry). Contact: HR lead | **Staff who don't sit at desks serve themselves.** Payslips and leave on WhatsApp or USSD, remittance with receipts, which takes payslip requests and agency filing off whoever fills `{{job_title}}`. Proof: Heroshe. PAYE-season line on | LIVE |
| KE-A | KE + S1 + 0 HR | As NG-A, in Kenya. Contact: founder or CEO | As NG-A, with `{{local_filings}}` for the filing list. No PAYE-season line. No local proof | HOLD |
| KE-B | KE + S1 + 1+ HR | As NG-B, in Kenya. Contact: HR lead | As NG-B, with `{{local_filings}}`. No PAYE-season line | HOLD |
| KE-C | KE + S2 + 0 HR | As NG-C, in Kenya. Contact: MD | As NG-C (WhatsApp and USSD launched in Kenya), with `{{local_filings}}`. No PAYE-season line | HOLD |
| KE-D | KE + S2 + 1+ HR | As NG-D, in Kenya. Contact: HR lead | As NG-D, with `{{local_filings}}`. No PAYE-season line | HOLD |
| GH-A | GH + S1 + 0 HR | As NG-A, in Ghana. Contact: founder or CEO | As NG-A, with `{{local_filings}}`. No PAYE-season line. No local proof | HOLD |
| GH-B | GH + S1 + 1+ HR | As NG-B, in Ghana. Contact: HR lead | As NG-B, with `{{local_filings}}`. No PAYE-season line | HOLD |
| GH-C | GH + S2 + 0 HR | As NG-C, in Ghana. Contact: MD | As NG-C (WhatsApp and USSD launched in Ghana), with `{{local_filings}}`. No PAYE-season line | HOLD |
| GH-D | GH + S2 + 1+ HR | As NG-D, in Ghana. Contact: HR lead | As NG-D, with `{{local_filings}}`. No PAYE-season line | HOLD |

**Branch coverage.** Every cell has `{{job_title}}` and `{{payroll_duty}}` (signals 1, 2 and 5 are required to enter). Cells A and C have no HR lead, so their angle speaks to the founder or MD about the handover; B and D speak to HR about the role being added. No KE or GH cell uses Nigerian rules. Funding never carries an angle.

**Niche-native test.** Each angle is tied to one moment: a company hiring someone to do `{{payroll_duty}}`. A and C differ by workforce (office versus shift and site); B and D differ the same way.

**Facts the angles depend on (all Pending in facts.md):** remittance after each run with receipts; WhatsApp and USSD access; attendance flowing into payroll (added 2026-11-17); FAMASI and Heroshe proof; the 2026 tax-law line. KE and GH need `{{local_filings}}` approved per country.

## 6. Variables contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{first_name}}` | People table: first name | All |
| `{{company_name}}` | Companies table: cleaned company name | All |
| `{{job_title}}` | Signal 1: job title, cleaned (no codes or locations) | All |
| `{{payroll_duty}}` | Signal 5: one matched duty | All |
| `{{round_type}}` | Signal 6: round type, only when signal 6 is true | S1 cells (NG-A, NG-B, KE-A, KE-B, GH-A, GH-B) where funding fired |
| `{{local_filings}}` | facts.md, once a KE or GH line is approved | KE and GH cells only (HOLD) |

**Routing fields, never shown in copy:** `country`, `segment`, `hr_team`, `paye_season` (switches the NG timing line on or off), `job_url` (for QA only).

**Not allowed in copy:** raw HR headcount, raw job-post dates, funding amounts, anything not in this table. Sender name and signature come from facts.md (open question).

## 7. Cadence and channel [ENGINE]

- **Periodicity:** always-on. Weekly job-post pull (last 30 days), deduped at company level; a company re-enters only on a new signal (gtm-system.md step 2).
- **Channel:** email plus LinkedIn (gtm-system.md step 9).
  - Email in PlusVibe: step 1 day 0, step 2 day 3 to 4, step 3 day 7 to 10, optional step 4 day 14+.
  - LinkedIn by a rep from HubSpot tasks: connection request with no note 1 to 2 days after email 1; DM 1 after acceptance; DM 2 three to five days later.
  - No valid email: LinkedIn-only track.
  - A reply on either channel pauses the other (n8n).
- **Timing line:** the 31 January 2027 return line runs in NG cells until 2027-01-15, then switches off.
- **Before launch:**
  - Test pull for job-post coverage.
  - facts.md approvals and a named sender.
  - `/campaign-qa` passes on the Clay table and the PlusVibe campaign.

---

## Copy

Pending. Written by `/campaign-copy` from the angles above and appended here.
