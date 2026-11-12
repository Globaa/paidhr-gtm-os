# PaidHR GTM Context OS

## Company
PaidHR (paidhr.com, formerly Pade HCM): Lagos-based full-suite HRMS for African employers. Covers payroll, automatic PAYE/pension/NHF/NSITF remittance, HR, hiring, attendance, performance, cross-border payroll, and employee financial wellness (earned wage access, Loan Marketplace, wallet). Core market is Nigeria (also Kenya, Ghana, South Africa). Core customers have 20 to 200 staff; the company is moving up to 100+. Founded by Seye Bandele (CEO) and Lekan Omotosho (CTO); $1.8M seed led by Accion Venture Lab (June 2025). Sources: `context/docs/research/company.md`. Summary profile: `context/icp/icp.md`, `context/positioning/positioning.md`.

## How this folder works
- `.claude/skills/`: reusable Claude skills for repeat GTM tasks. Read when a task matches a skill.
- `context/icp/`: ideal customer profiles and buyer personas. Read before any targeting, list building or copy.
  - `icp.md`: five-layer ICP and who is not a fit.
  - `personas.md`: champion, economic buyer, influencer.
  - `signal-matrix.md`: segments x personas with angles, scored buying signals, top five to build, workflow tracker (DRAFT).
- `context/positioning/`: category, differentiators and competitor comparisons. Read before writing any claim about PaidHR.
  - `positioning.md`: category, target, alternative, unique value, proof, statement.
  - `value-proposition.md`: job, pain, gain, proof per persona.
  - `competitors.md`: competitor table, where we win and lose, responses.
- `context/messaging/`: approved value props, pain points and proof points. Read before writing any copy.
  - `messaging.md`: core message, pillars, persona angles, words to use and avoid.
  - `facts.md`: the only allowed source of names, numbers and claims for prospect copy (all claims Pending approval).
- `context/brand-kit/`: voice, tone, visual rules and assets. Read before producing anything customer-facing.
  - `brand.md`: tone of voice, key phrases, names, values, visual basics.
- `context/docs/`: company documents.
  - `case-studies.md`: every customer story found (FAMASI, Heroshe) plus logo-only customers.
  - `gtm-system.md`: the outbound GTM system end to end (12 steps, tools, decisions, four-pillar check, Miro board link). DRAFT.
- `context/docs/research/`: raw research with sources (site, reviews, news, competitors). Read when a fact needs a citation.
  - `company.md`, `customers.md`, `customer-videos.md`, `competitors.md`, `market.md`, `signals.md`, `voice.md`.
- `knowledge-base/playbooks/`: step-by-step GTM workflows that work. Read before running or designing a motion.
- `knowledge-base/learnings/`: results, experiments and what we learned. Read before repeating or changing a motion.
- `meetings/`: meeting notes and call transcripts, one file per meeting. Read for decisions and customer voice.
- `output/campaigns/`: finished campaign assets and copy, one folder per campaign. Read to reuse or iterate.
- `output/outbound/`: outbound sequences, emails and LinkedIn messages. Read before writing new outbound.
- `output/social/`: social posts and content drafts. Read before writing new social content.
- `output/data/`: lists, enrichment outputs and analysis (CSV/XLSX are gitignored). Read when building or reusing a list.
- `inspiration/`: ads and examples to learn from, saved with source and a note on why it works. Read before creating any new asset.
- `diagrams/`: flowchart specs (JSON) and generated SVGs for Miro boards (`miro-gtm-flowchart` skill). Edit the JSON and rebuild; don't hand-edit the SVG.
- `tools/`: scripts and tool configs. Read when automating or connecting a tool.
- `weekly-priorities/`: one file per ISO week (YYYY-Www.md) with the week's priorities. Read at the start of every session.
- `work-log/`: one file per ISO week with timestamped change entries. Write to it after every change.

## Rules
- Cite a source (URL or file path) for every company fact.
- Never invent numbers. If a figure is unknown, write "unknown" and flag it.
- No em dashes in any copy.
- Ask before sending or publishing anything.
- Log every change in this week's work log (`work-log/YYYY-Www.md`) with a timestamp.

## Skills
Project skills in `.claude/skills/`, from github.com/bolaji-ships/gtm-starter-kit (commit 0e6d90b). Supporting playbooks are in `knowledge-base/playbooks/`, supporting tools in `tools/`.
- Campaign pipeline, run in this order: `signal-matrix` (ICP segments and the signals that find them) → `campaign-strategy` (15-25 outbound campaign ideas) → `build-workflow-spec` (turn one idea into an approved workflow spec) → `campaign-copy` (cold email and LinkedIn DM copy from an approved spec) → `campaign-qa` (check a Clay table or Instantly/PlusVibe campaign against the spec).
- Clay build: `campaign-contract` (build sheet for one Clay campaign) → `design-clay-table-json` (table blueprint JSON) → `import-clay-table` (import and test-run in Clay within budget) → `save-clay-schema` (save an approved table to the registry). `build-clay` builds or audits a full Companies + People workbook. `prompt-builder` writes per-row AI prompts for Clay columns.
- Clay costs and data: `calculate-costs` (spend per table and column), `workspace-cost-audit` (monthly workspace spend), `export-tables` (Clay tables to CSV).
- Deliverability: `inbox-placement-test` (EmailBison + MailReach inbox placement test per sender pool).
- Web and visuals: `crawl4ai` (crawl sites to Markdown/JSON locally), `agent-browser` (scripted browser automation), `miro-gtm-flowchart` (draw a GTM motion as a Miro flowchart).

## Tools
- treg (treg.to, CLI `treg`): paid data lookups (enrichment, SERP, ads, social, web). Team: toptal. Project config at `.treg/config.json` (via `TREG_CONFIG` in `.claude/settings.local.json`). Skills: `treg`, `lead-signals`, `web-access`, `make-ugc`, `recruit`. Check `treg balance` and the tool's price before every call.
- Miro (MCP, plugin `miro@miro-ai`, project scope): diagrams and visual boards (flows, maps, architecture).
- Notion (MCP server `notion`, declared in `.mcp.json`): where we publish finished docs. GitHub stays the source of truth: write and edit in this repo first, then publish to Notion. Ask before publishing.
