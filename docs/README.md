# Store Stock Management documentation index

Authority order when documents disagree: **Leo's recorded decisions ([DECISIONS](DECISIONS.md)) > [BUSINESS-RULES](BUSINESS-RULES.md) > other documents in this folder > the mockup.** The mockup (`dough-planner-simple.html`) is throwaway and is not a requirement.

Tags used in the specification:

| Tag | Meaning |
| --- | --- |
| **A** | Approved by Leo (in the planning chat; the date is in DECISIONS) |
| **P** | Proposed by Claude, not yet approved. Nothing marked P is built until Leo approves it |
| **?** | Open question, listed in [DECISION-GATE](DECISION-GATE.md). Until answered, the feature is not built or is denied |

The structure follows the TDMS documentation set, in a shorter form.

## Working documents

| Doc | Purpose |
| --- | --- |
| [PROJECT-STATE](PROJECT-STATE.md) | Current phase, blockers, next step |
| [DECISIONS](DECISIONS.md) | Approved decisions D001..; long form in [adr/](adr/) |
| [DECISION-GATE](DECISION-GATE.md) | Open gates G-01.. and the answers recorded |
| [CHANGELOG](CHANGELOG.md) | Change history of these documents |
| [PLANNING-NOTES](PLANNING-NOTES.md) | Planning history and the facts read from the store workbook |
| [CLAUDE.md](../CLAUDE.md) | Rules for Claude Code in this repository |

## Specification

| Doc | Purpose |
| --- | --- |
| [PROJECT-BIBLE](PROJECT-BIBLE.md) | What the system is, the business problem, the value, scope and permanent rules |
| [REQUIREMENTS](REQUIREMENTS.md) | Requirements REQ-001.. |
| [BUSINESS-RULES](BUSINESS-RULES.md) | Business rules RULE-001.. and settings CFG-001.. |
| [DATA-MODEL](DATA-MODEL.md) | Logical data model (schema authority) |
| [ARCHITECTURE](ARCHITECTURE.md) | Layers, modules, request flow, repository layout, limits |
| [PERMISSIONS](PERMISSIONS.md) | Roles and what each may do |

## Decision records

| Record | Topic |
| --- | --- |
| [ADR-0001](adr/0001-cloudflare-worker-d1-pwa.md) | Platform: Cloudflare Worker, D1 and an installable PWA |
| [ADR-0002](adr/0002-item-rules-and-pd-tracking.md) | Item rules and production-date tracking |

## Planned for the next batch (not written yet)

WORKFLOWS (receiving, daily count, plan approval), CONFIGURATION (full settings blueprint), API-CONTRACT, TEST-STRATEGY, ROADMAP (phases and slices), HANDOFF-MODEL (task packet for Claude Code).

## Source material

- Workbook `AP MALATE FC P12W2.xlsx` (one store, one week). Facts read from it are in [PLANNING-NOTES](PLANNING-NOTES.md) sections 3b and 3d.
- Mockup: `../design/dough-planner-simple.html` (throwaway design reference; also published as a private artifact).
- Project plan for management reading: the Docs artifact "Store Stock Management: Project Plan". This folder is the working source; the plan document is updated when decisions here change.
