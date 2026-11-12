# PaidHR Outbound GTM System

**Status: DRAFT design.** Nothing here is built yet. Built from `context/icp/` (icp.md, personas.md, signal-matrix.md), `context/messaging/facts.md` and the playbooks in `knowledge-base/playbooks/`. Last updated 2026-11-12.

**Miro board:** [PaidHR GTM System](https://miro.com/app/board/uXjVEO4MgMI=/)

**Rules this system follows**
- Every claim in prospect copy comes from `context/messaging/facts.md`. Every claim there is still Pending, so **no sequence can send until a PaidHR owner approves facts.md.**
- No numbers are invented. Thresholds below are either benchmarks from a cited playbook or labelled as a **starting rule** to test and change.
- **INFERRED** means the research suggests it but does not state it.
- Nothing is sent or published without approval (CLAUDE.md).

---

## The system at a glance

| # | Step | Tool | Output |
|---|---|---|---|
| 1 | Context OS | Claude Code, GitHub | Approved ICP, signals, facts and workflow spec |
| 2 | Buying signals | Clay (n8n schedules runs) | Raw signal rows: company, signal, date, source URL |
| 3 | Find companies | Clay | One row per company with a clean domain |
| 4 | Enrich | Clay | Headcount, industry, HQ, states, funding, ERP mentions |
| 5 | ICP fit check | Clay | Pass, drop, or hold |
| 6 | Route by segment and angle | Clay | Segment S1 to S4, plus angle |
| 7 | Find people | Clay | 1 to 2 contacts per account, by persona |
| 8 | Find and verify email | Clay, HubSpot (check) | Email track, LinkedIn-only track, or excluded |
| 9 | Sequence | PlusVibe (email), HubSpot tasks (LinkedIn), n8n | Contacts in a live campaign |
| 10 | Reply handling | PlusVibe, n8n, Claude | Every reply tagged and routed |
| 11 | CRM | HubSpot | Deals with signal, segment and angle attached |
| 12 | Weekly feedback loop | n8n, Claude Code, GitHub | Updated Context OS and a learnings note |

Tools: the six in the brief. Where the stack has no tool for a job (automated LinkedIn sending), the doc says so and uses the closest fit.

---

## Step 1: Context OS

**What happens.** Every campaign starts in this repo. Claude Code reads the Context OS and runs the campaign pipeline in order: `signal-matrix` (done, DRAFT) → `campaign-strategy` → `build-workflow-spec` → `campaign-copy` → `campaign-qa`. GitHub holds every version. Notion gets finished docs only after approval.

**Tool.** Claude Code (skills), GitHub (source of truth and history).

**Inputs it reads.** icp.md (segments, not-a-fit list), personas.md, signal-matrix.md (signals W1 to W9), facts.md (allowed claims), messaging.md, brand.md, this week's `weekly-priorities/` file, and last week's `knowledge-base/learnings/` note.

| Decision | Answer → what happens |
|---|---|
| Is the workflow spec saved as STRUCTURE-APPROVED? | Yes → Clay build can start (`campaign-contract`, `design-clay-table-json`, `import-clay-table`). No → stop. No Clay spend before the spec is approved |
| Is every claim the copy needs Approved in facts.md? | Yes → copy can be written for sending. No → copy can be drafted with `{{placeholders}}` but **cannot send**. Today the answer is No for every claim |
| Is a sender chosen (name, title, signature)? | Yes → copy and PlusVibe set-up continue. No → stop at copy. Open question in facts.md |
| Which workflow is this week's priority? | Read from `weekly-priorities/`. Default order from signal-matrix.md: W1, W2, W3, W4, W5 |

---

## Step 2: Buying signals

**What happens.** Clay pulls signal rows on a schedule. The signal comes first and the ICP filter second (framework guardrail: "the signal IS the trigger"). Each workflow has one mother intent (workflow-anatomy.md).

**Tool.** Clay (sources and HTTP calls; treg endpoints from signal-matrix.md where Clay has no source). n8n runs the schedule.

**Signals live at launch** (from signal-matrix.md, top five):

| W | Signal | Source | Run frequency (workflow-anatomy.md periodicity) |
|---|---|---|---|
| W1 | Hiring HR manager, HR officer, payroll officer, People Ops (U1), with "manual payroll skills" stacked (U2) | Job posts, Nigeria, last 30 days | Weekly |
| W2 | Frontline hiring: 3+ rider, driver, warehouse or store posts in 60 days (N1) | Job posts | Weekly |
| W3 | Nigeria-HQ company hiring in Kenya, Ghana or South Africa (U6) | Job posts with location filter | Weekly |
| W4 | Firm advertises payroll services for clients (N4), timed by payroll-officer hiring (U1) | Directories, firm websites, job posts | Monthly list, weekly hiring check |
| W5 | New HR or finance leader, last 90 days (U3) | People search by start date | Monthly |

W6 to W9 (multi-state, ERP in job text, headcount growth, funding) are added as enrichments in step 4, not as separate pulls, until W1 to W5 have results.

| Decision | Answer → what happens |
|---|---|
| Did the signal fire inside its window (30, 60 or 90 days)? | Yes → keep the row. No → discard |
| Has this company already entered this workflow? | No → continue. Yes, same signal → skip (dedupe). Yes, but a new signal → re-enter with the new signal as the reason |
| Does the source return a company name or domain? | Yes → step 3. No → discard (job posts from staffing agencies hiring for an unnamed client fall here) |
| **Before the first build:** do the job providers cover Nigerian boards (Jobberman, LinkedIn Jobs)? | Yes → W1, W2, W3 go ahead. No → W1 to W3 lose their data source; switch to W4 and W5 first and find another job source. Checked by one small test pull (signal-matrix.md, "Before any build") |

---

## Step 3: Find companies

**What happens.** Signal rows become company rows. One Clay table per data source, then one consolidated Companies table (Clay iron laws 4 and 5: one table per source, accounts separate from contacts).

**Tool.** Clay. HubSpot is read through Clay's HubSpot integration for the account check.

| Decision | Answer → what happens |
|---|---|
| Clean domain found? | Yes → continue. No → one Claygent lookup (one task per run, iron law 11). Still no → drop |
| Company already in HubSpot as a customer? | Yes → exclude from outbound; flag to the account owner if the signal suggests expansion (for example, hiring in a new country). No → continue |
| Open deal or active sequence on this company in HubSpot? | Yes → exclude; add the signal as a note on the deal for the owner. No → continue |
| Company is a known competitor (SeamlessHR, Workpay, NotchHR, Eazipay, Bujeti, Salario)? | Yes → drop. No → continue |
| Duplicate row in the consolidated table? | Yes → merge, keep every signal on one row (iron law 13). No → continue |

---

## Step 4: Enrich

**What happens.** Account-level enrichment only, and only on rows that passed step 3 (filter before enriching, actions-data-credits.md). Own API keys first, Clay credits second (iron law 9). Every new column is tested on 10 rows before the full table (iron law 7).

**Tool.** Clay (Apollo/LinkedIn company data, Claygent, HTTP calls to treg endpoints such as Google Maps search and funding rounds).

**Fields:** headcount band, LinkedIn industry, HQ country and city, number of Nigerian states with branches (Google Maps, N2), latest funding round and date (S1 only, U5), ERP named in finance job posts (N3), offices or hiring outside Nigeria (U6), website text for payroll-services wording (N4).

| Decision | Answer → what happens |
|---|---|
| Headcount known? | Yes → continue. No → Claygent reads the website and LinkedIn page. Still unknown → "Needs review" view, reviewed weekly by a person. Not sent |
| Logistics, retail or manufacturing company with LinkedIn headcount under 20? | Check job-post volume and branches before dropping. Frontline staff are rarely on LinkedIn, so LinkedIn undercounts these firms (INFERRED, signal-matrix.md). If 3+ frontline posts or 2+ branches → treat as 20+ and continue. Otherwise → step 5 drops it |
| Spend for this batch inside the budget set in the workflow contract? | Yes → run. No → stop and check with `calculate-costs` before topping up |

---

## Step 5: ICP fit check

**What happens.** A formula column in Clay gives each company Pass, Drop or Hold. Conditional runs, not filters, control what runs next (iron law 12).

**Tool.** Clay.

| Decision | Answer → what happens |
|---|---|
| HQ or main operation in Nigeria? | Yes → continue. Kenya, Ghana or South Africa → Hold list (not researched yet, icp.md). Elsewhere → Drop |
| 20 or more employees (after the step 4 frontline check)? | Yes → continue. Under 20 → Drop from outbound (no tier below 20; Salario is free there). Kept in a list for content, not sales |
| On the not-a-fit list: wants fully outsourced payroll or EOR as a buyer, or wants payroll inside a spend tool? | Detected from website text where possible. Yes → Drop. No or unknown → continue |
| In the LSETF free-access cohort? | Cannot be checked yet (names not public). Ask PaidHR for the list; when it exists, Yes → exclude from cold outbound and hand to PaidHR's own team |

---

## Step 6: Route by segment and angle

**What happens.** Each passing company gets one segment (signal-matrix.md, Matrix 1), then an angle from its secondary signals. Segments are mutually exclusive, so the order of checks matters.

**Tool.** Clay (formula columns, then a Claygent column only where text has to be read).

**Segment routing, checked in this order:**

| Decision | Answer → what happens |
|---|---|
| Does the company sell payroll administration, payroll outsourcing or staffing to clients? | Yes → **S4 Payroll partners** (white-label). No → next check |
| 200 or more employees? | Yes → **S3 Large employers**. No → next check |
| 20 to 199, industry Software, Financial Services, IT Services or Internet? | Yes → **S1 Funded tech/fintech** (H1) |
| 20 to 199, industry Logistics, Retail, Manufacturing, Food and Beverage, Warehousing? | Yes → **S2 Frontline operators** (H2) |
| 20 to 199, any other industry? | → "Unrouted" view. Not sent. Reviewed weekly; if many good companies collect here, it becomes a new segment in signal-matrix.md |

**Angle routing inside a segment** (a conditional waterfall: the first matching line wins, copy-iron-laws.md law 2). Angles come from Matrix 1 and must use approved facts only.

| Secondary signal present? | Angle line used |
|---|---|
| Hiring in a second African country (U6) | Cross-border payroll |
| Job post asks for manual payroll skills (U2) | The new hire's PAYE and pension remittance workload |
| New HR or finance leader (U3) | First 90 days in the role |
| Branches in 2+ states (N2) | Remittance to each state |
| ERP named in finance posts (N3, S3 only) | "Your ERP stays" |
| Recent round (U5, S1 only) | Growth after the raise |
| None of the above | The mother signal alone (for W1: the role they are hiring for) |

Until 31 January 2027, every route can add the annual PAYE return as a timing line (signal-matrix.md, N9).

---

## Step 7: Find people

**What happens.** A separate People table, filled by lookup from Companies (accounts are not contacts, iron law 5). Personas come from personas.md, adapted per segment.

**Tool.** Clay (Find People, Apollo people search via own key).

| Segment | Champion (contact first) | Economic buyer | Influencer |
|---|---|---|---|
| S1 | HR Manager, People Ops lead | Founder / CEO | Finance Manager |
| S2 | HR Manager, HR Officer | Managing Director / owner | Finance Manager, Accountant |
| S3 | Head of HR / HR Director | CFO | Financial Controller, Payroll lead |
| S4 | Head of payroll services | Managing Partner / owner | Tax or compliance lead |

| Decision | Answer → what happens |
|---|---|
| Did U3 fire (new leader)? | Yes → that person is the contact, whatever the persona. No → next check |
| Champion found? | Yes → champion is contact 1, economic buyer is contact 2. No → economic buyer is contact 1 (FAMASI shows founders run payroll in small firms, customer-videos.md) |
| More than 2 contacts at the account? | Keep 2, one per persona, and send them different angles. **Starting rule** to test; iron law 14 says never send to everyone at one account |
| Nobody found for any persona? | Account goes to "No contact" view. Re-checked next month. Not sent |

---

## Step 8: Find and verify email

**What happens.** Clay runs an email waterfall (own keys first), then a verification step. HubSpot is checked again at contact level.

**Tool.** Clay (waterfall plus verification), HubSpot (contact check).

| Decision | Answer → what happens |
|---|---|
| Work email found? | Yes → verify. No → LinkedIn-only track (step 9) |
| Verification result | Valid → email track. Catch-all or risky → LinkedIn-only track (protects the bounce rate). Invalid → LinkedIn-only track |
| Contact already in HubSpot? | Unsubscribed or marked do-not-contact → exclude everywhere. In another active sequence → exclude. Contacted inside the suppression window → exclude (window: **starting rule** of 90 days, to agree with PaidHR). Not in HubSpot or outside the window → continue |
| LinkedIn profile URL found? | Yes → can also get LinkedIn touches. No and no valid email → drop the contact |

---

## Step 9: Sequence (email and LinkedIn)

**What happens.** Copy is written per segment and angle with `campaign-copy`, from the approved spec and approved facts only. `campaign-qa` checks the Clay table and the PlusVibe campaign against the spec before launch. Clay pushes contacts into PlusVibe. n8n creates LinkedIn tasks in HubSpot.

**Tool.** PlusVibe (email sending), HubSpot tasks (LinkedIn, sent by a rep), n8n (pushes, pauses, task creation), Clay (push to PlusVibe), Claude Code (`campaign-copy`, `campaign-qa`).

**Email cadence** (copy-frameworks.md, Follow-Up Stack): Step 1 day 0, observation from the signal. Step 2 day 3 to 4, new value. Step 3 day 7 to 10, new angle or "right person?". Optional step 4 day 14+, breakup or referral. Each follow-up adds something new. 60 words is the target, 120 the ceiling (copy-iron-laws.md law 5).

**LinkedIn cadence** (linkedin-dm-frameworks.md): connection request with no note on day 1 to 2 after email 1. DM 1, one question, 1 to 2 days after acceptance. DM 2, context and a soft offer, 3 to 5 days later. No third DM. At most 20 to 30 requests per day per account. For W5 (new leader), LinkedIn leads and email supports (same playbook, priority sequencing).

**Gap:** the stack has no LinkedIn sending tool, so a rep sends from HubSpot tasks by hand. Adding an automation tool is an open decision.

| Decision | Answer → what happens |
|---|---|
| `campaign-qa` passed? | Yes → launch. No → fix in Clay or copy, re-run QA |
| Track | Email track → PlusVibe sequence plus LinkedIn tasks. LinkedIn-only track → HubSpot tasks only |
| Reply on either channel? | Yes → n8n pauses the other channel at once, step 10. No → sequence continues |
| Connection accepted within 2 weeks? | Yes → DM 1. No → stop LinkedIn for this contact |
| Bounce rate over 3% on a campaign (copy-frameworks.md target)? | Yes → n8n pauses the campaign; fix the list and verification before restarting. No → keep sending |

---

## Step 10: Reply handling

**What happens.** PlusVibe sends each reply to n8n by webhook. Claude (via n8n) tags the intent. n8n acts on the tag. LinkedIn replies are logged by the rep in HubSpot and follow the same table.

**Tool.** PlusVibe (reply webhook), n8n (routing), Claude (classification), HubSpot (actions).

| Reply tagged as | What happens |
|---|---|
| Interested / wants a demo | Create HubSpot deal at "Interested", alert the owner, stop all sequences for the account |
| Not now, gives a time ("after January") | HubSpot task on that date; contact leaves the sequence |
| Wrong person, names someone | New contact goes back to step 7 with the referral as the reason |
| Uses another tool already | Record the competitor on the HubSpot company; stop; log for the weekly review |
| Objection (price, trust after Bento, where money sits) | Owner answers by hand from approved responses. Never answer automatically. The objection is logged for the weekly review |
| Unsubscribe or hostile | Suppress in PlusVibe and HubSpot at once |
| Out of office | Pause; resume after the return date |
| No reply after the last step | Contact ends. Company returns to the signal pool and only re-enters on a new signal (step 2 dedupe rule) |
| Classifier unsure | Goes to a person to tag by hand. Never auto-acts on a low-confidence tag |

---

## Step 11: CRM

**What happens.** HubSpot is the system of record for companies, contacts, deals and activity. Every record carries the fields the feedback loop needs.

**Tool.** HubSpot. Clay and n8n write to it.

**Required properties on company, contact and deal:** workflow ID (W1 to W9), mother signal, signal date, segment (S1 to S4), angle, persona, sequence name and version, source table.

**Deal stages** (starting set; PaidHR's current stages are unknown): Interested → Meeting booked → Demo held → Proposal sent → Closed won / Closed lost.

| Decision | Answer → what happens |
|---|---|
| Meeting held? | Yes → Demo held. No-show → rebook task for the owner |
| Proposal accepted? | Yes → Closed won; the customer goes on the customer list that steps 3 and 8 exclude, and becomes a lookalike seed (U8). No → Closed lost with a reason from a fixed list (price, competitor, timing, no fit, no response) |
| Closed lost for timing? | Yes → task to re-check at the given date; company back in the signal pool. Other reasons → logged for the weekly review |

---

## Step 12: Weekly feedback loop

**What happens.** Every week n8n pulls campaign numbers from PlusVibe and pipeline numbers from HubSpot, by workflow, segment, angle and persona. Claude Code reads them, writes `knowledge-base/learnings/YYYY-Www.md`, proposes edits to the Context OS, and logs the change in `work-log/`. A person approves; GitHub commits.

**Tool.** n8n (pull), Claude Code (analysis and edits), GitHub (commit), HubSpot and PlusVibe (data).

**Benchmarks to compare against** (copy-frameworks.md, generic, not PaidHR results): reply rate 5 to 15%, positive reply rate 2 to 8%, bounce under 3%, unsubscribe under 0.5%.

| Finding | What changes |
|---|---|
| Bounce over 3% | Tighten verification in step 8; check sending set-up with `inbox-placement-test` |
| Opens fine, replies low | Rewrite copy for that angle (`campaign-copy`), keep the list |
| One segment beats another on positive replies (S1 vs S2) | Update icp.md: H1 or H2 moves towards "core". This is the answer to the ICP's top open question |
| A signal produces no meetings after a fair sample | Mark it KILLED in signal-matrix.md with the reason. Sample size: **unknown**, to agree with PaidHR before launch |
| New objection appears 2+ times | Add a response to messaging.md and, once approved, to facts.md |
| Many companies in "Unrouted", "Needs review" or Hold | Propose a new segment or rule in signal-matrix.md |
| Closed-won pattern (industry, size, signal) | New lookalike seeds; re-score signals |

---

## Four pillars check

| Pillar | Where the design meets it | Gaps to close |
|---|---|---|
| **Clean data** | One table per source plus one consolidated table; dedupe at company and contact level; HubSpot checked twice (account in step 3, contact in step 8); email verification with catch-all sent to LinkedIn; required HubSpot properties on every record; frontline headcount check so S2 is not wrongly dropped | HubSpot properties and deal stages don't exist yet. PaidHR's current CRM data quality is unknown. Customer and LSETF lists are needed for exclusions |
| **Always-on signals** | Each workflow is a scheduled pull (weekly for job signals, monthly for leadership and directories), set by n8n; new signals re-open accounts that went quiet | Nigerian job-board coverage is unproven (one test pull decides it). Only W1 to W5 are live at launch |
| **Composable workflows** | One U1 hiring build serves all four segments with keyword swaps; the same People, email and HubSpot-check blocks are reused by every workflow (Clay templates, iron laws 17 and 18); approved tables saved with `save-clay-schema`; copy routed by a waterfall, not one-off lists | The block templates have not been built. The first workflow (W1) should be built as the template for the rest |
| **Tight feedback loops** | Every record carries workflow, signal, segment, angle and persona, so results trace back; weekly review writes learnings and edits the Context OS; reply objections feed messaging; closed-won feeds lookalikes | Copy cannot send until facts.md is approved, so there is no data yet. LinkedIn replies depend on reps logging them by hand. The sample size for killing a signal is not agreed |

**Verdict:** the design covers all four pillars. The real blockers are not design gaps but approvals and set-up: facts.md sign-off, a sender, HubSpot properties, the Nigerian job-board test pull, and a decision on LinkedIn sending.

---

## Open decisions for PaidHR
1. Approve facts.md and choose a sender (blocks all sending).
2. HubSpot: create the properties and stages above, or share the current set-up.
3. LinkedIn sending: rep by hand from HubSpot tasks (current design) or add an automation tool.
4. Suppression window and the sample size for killing a signal.
5. Customer list and LSETF cohort list for exclusions.
6. Sending domains and warm-up for PlusVibe (not covered by the research).

---

## Diagram
Miro board: [PaidHR GTM System](https://miro.com/app/board/uXjVEO4MgMI=/) (created 2026-11-12; 40 boxes in 3 rows, plus the weekly loop back to the Context OS)
Flowchart spec: [diagrams/paidhr-gtm-system.json](../../diagrams/paidhr-gtm-system.json)
