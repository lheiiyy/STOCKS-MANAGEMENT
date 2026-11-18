# STOCKS-MANAGEMENT - rules for Claude Code

Store stock management for Figaro Culinary Group stores. It forecasts how much dough, carbo and palabok a store should order or make, and on which day, so each item is used inside its usable days. Status: planning, no product code yet.

Map: STATUS = `docs/PROJECT-STATE.md` · decisions = `docs/DECISIONS.md` and `docs/adr/` · rules = `docs/BUSINESS-RULES.md` · open questions = `docs/DECISION-GATE.md` · index = `docs/README.md`

## Stack (ADR-0001, D017)

Cloudflare Worker and D1 (SQLite) on the free tier, React with Vite as an installable PWA, deployed from GitHub Actions. TypeScript is proposed (G-05). Nothing is scaffolded yet. A new dependency is a scope change and needs Leo's OK.

## Working with Leo

- Leo owns the project and approves every structural change, calculation rule and deploy. Plan -> Leo's OK -> build -> Leo reviews every PR -> deploy.
- Reply in Taglish. Write docs, specs and UI text in plain formal English.
- Leo is learning Git: create the branch, commit and open the PR for him, and explain in one line what changed. Never push to `main`. The first commit of the empty repository (README only, 2026-11-18) was the one exception. Leo merges every PR.
- Never commit credentials, tokens, Cloudflare account or database IDs, or `.env` files. Secrets live in GitHub and Cloudflare secrets. Only a root `.env.example` is allowed.
- Deploy to live only when Leo says so. No real store or employee data in development or test; test uses dummy data.
- Patches are surgical. Do not rewrite what was not asked.
- Needs a separate OK from Leo at the time: a schema change, a calculation or business rule change, a permission or role change, security impact, anything touching live, and any item still open in `docs/DECISION-GATE.md`.
- No product code, scaffolding or dependency install before Leo approves the design.

## Read order

`CLAUDE.md` -> `docs/PROJECT-STATE.md` -> the docs the task names -> affected source -> relevant tests. Nothing else.

## Authority

DECISIONS > BUSINESS-RULES > other documents in `docs/` > the mockup in `design/` (throwaway). `docs/DATA-MODEL.md` is the schema authority. Tags: A approved, P proposed, ? open. A rule tagged P or ? is not built without Leo's OK. Do not invent requirements.

## Layers (ARCHITECTURE section 1)

React screens -> API client (`mock` or `rest` transport) -> Worker routes (session, role check, validation, envelope) -> services -> pure rules (`packages/shared`) / repositories -> D1. A layer calls only the layers below it; nothing calls upward. Only repositories touch D1. Pure rules do no I/O and do not read the clock; the clock and ID generator are passed in. Every protected call checks session and role on the server. Every table has the audit columns, every change writes `audit_log` in the same transaction, and records are deactivated, never deleted.

## Skills

`/plan` before any slice -> wait for Leo's OK -> `/execute-plan` -> `/verify-before-complete`. `/tdd` for pure rules. `/review` before a PR. `/one-shot` is not used for schema, authentication, permissions, deploy or calculation rules. Use `superpowers:brainstorming` for anything the plan does not cover.

## Stop rule

On a missing or conflicting rule, an open gate, an undefined permission, a schema change beyond the plan, security impact or an unclear calculation: stop and report `BLOCKED — REASON — DECISION REQUIRED`. Do not guess or widen scope.

## Definition of done and commits

Implementation complete · tests pass · server-side authorization verified · validation and errors handled · browser check on phone, tablet and desktop widths · docs updated · no unrelated files changed · `docs/PROJECT-STATE.md` and `docs/CHANGELOG.md` updated. Feature branches only, one commit per milestone in the approved plan, citing the RULE or REQ ids.
