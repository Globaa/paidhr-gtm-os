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
  - `signal-matrix-framework.md`, `workflow-anatomy.md` (Mother Intent Catalog): segment and signal method.
  - `copy-iron-laws.md`, `copy-frameworks.md`, `linkedin-dm-frameworks.md`, `copy-bans.yaml`: copy rules and banned words.
  - `clay/`: `iron-laws.md`, `actions-data-credits.md`, `formulas.md`, `prompts.md`, `expert.md`.
  - `templates/facts-template.md`: template for `context/messaging/facts.md`.
- `knowledge-base/learnings/`: results, experiments and what we learned. Read before repeating or changing a motion.
- `meetings/`: meeting notes and call transcripts, one file per meeting. Read for decisions and customer voice.
- `output/campaigns/`: finished campaign assets and copy, one folder per campaign. Read to reuse or iterate.
  - `campaign-strategy.md`: 18 ranked outbound campaign ideas and the pick (campaign 1). DRAFT.
  - `hr-payroll-hiring-manual-payroll/workflow-spec.md`: campaign 1 spec and copy (3 emails, 2 LinkedIn DMs). COPY-APPROVED 2026-11-17, Nigeria cells live, Kenya and Ghana on hold, not sendable until facts are approved and a sender is named.
  - `new-hr-finance-leader/workflow-spec.md`: campaign 4 spec (new HR or finance leader in the last 90 days, Nigeria, 7 cells). STRUCTURE-APPROVED 2026-11-18, copy pending.
- `output/outbound/`: outbound sequences, emails and LinkedIn messages. Read before writing new outbound.
- `output/social/`: social posts and content drafts. Read before writing new social content.
- `output/data/`: lists, enrichment outputs and analysis (CSV/XLSX are gitignored). Read when building or reusing a list.
- `inspiration/`: ads and examples to learn from, saved with source and a note on why it works. Read before creating any new asset.
- `diagrams/`: flowchart specs (JSON) and generated SVGs for Miro boards (`miro-gtm-flowchart` skill). Edit the JSON and rebuild; don't hand-edit the SVG.
  - `paidhr-gtm-system.json` (spec) and `paidhr-gtm-system.svg` (generated): the GTM system flowchart on the Miro board.
- `tools/`: scripts and tool configs. Read when automating or connecting a tool.
  - `agent-browser/`, `crawl4ai/`: READMEs for the matching skills.
  - `rex/`: unofficial Python client for Clay's internal API (uses your browser session, no API key). Used by the Clay build and cost skills. Not affiliated with Clay; may break.
- `weekly-priorities/`: one file per ISO week (YYYY-Www.md) with the week's priorities. Read at the start of every session.
  - `2026-W45.md`: first week's list (all done) and what's next. No file yet for 2026-W47.
- `work-log/`: one file per ISO week with timestamped change entries. Write to it after every change.
  - `2026-W45.md`, `2026-W47.md`.
- Root config:
  - `.mcp.json`: Notion MCP server (project scope).
  - `.claude/settings.json`: Miro plugin and marketplace (project scope).
  - `.gitignore`: keeps secrets and data out of Git.
  - Gitignored, never commit: `.env` (treg token), `.treg/` (treg project config), `.claude/settings.local.json` (`TREG_CONFIG`), CSV/XLSX in `output/data/`.

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
- Miro (MCP, plugin `miro@miro-ai`, project scope): diagrams and visual boards (flows, maps, architecture). Board: [PaidHR GTM System](https://miro.com/app/board/uXjVEO4MgMI=/).
- GitHub (source of truth): public repo https://github.com/Globaa/paidhr-gtm-os. Work is on branch `context-os-v1` (not merged to `main` yet). `gh` CLI is logged in as Globaa; plain `git push` has no stored credentials, so push with `git -c credential.helper= -c 'credential.helper=!gh auth git-credential' push`. The repo is public: scan for keys and personal paths before every commit.
- Notion (MCP server `notion`, declared in `.mcp.json`): where we publish finished docs. GitHub stays the source of truth: write and edit in this repo first, then publish to Notion. Ask before publishing.
  - Published mirror: [PaidHR GTM OS](https://app.notion.com/p/41b62bcf06ca816b829ccaba81a8420f) (under the private page "PaidHR"). Child pages mirror `context/` (ICP, Positioning, Messaging, Brand, Docs) and `output/campaigns/`; raw research is not published. Each page starts with "Source of truth: GitHub" plus the repo link and file path. Last published 2026-11-17 from branch `context-os-v1`. Republish a page after its file changes. Known issue: Notion auto-links `.md` file names (for example to http://facts.md).
- Planned outbound stack (from `context/docs/gtm-system.md`, **not connected to this repo yet**): Clay (signals, enrichment, people, email waterfall; driven by the Clay skills and `tools/rex`), HubSpot (CRM, exclusions, LinkedIn tasks), n8n (schedules, reply routing, weekly data pull), PlusVibe (email sending). No LinkedIn sending tool chosen yet.
