# ADR-0001 · Platform: Cloudflare Worker, D1 and an installable PWA

- **Status:** Accepted for the platform (Leo's hosting answer, D003) and for React as the front end (D017). The language (TypeScript) and library choices are proposed and tracked in G-05.
- **Decider:** Leo. **Date:** Nov 2026.

## Context

Leo wants a stock app on a free tier, built the way he built the Liquidation form app. The company has no budget for a server. The data is small: a few items per store, daily counts and a few batches. He works from an Android tablet.

## Decision

| Area | Choice |
| --- | --- |
| Compute | Cloudflare Worker on Leo's own account |
| Database | D1 (SQLite) on the free tier; plain SQL migrations kept in the repository |
| Front end | React with Vite, built as an installable PWA and served by the Worker (D017) |
| Deploy | GitHub Actions with the Cloudflare API token and account ID as repository secrets |
| Layers | screens, API client (mock and rest transport), services, repository, database |

## Facts checked

D1 free limits on Cloudflare's own page: 5 million rows read per day, 100,000 rows written per day, 5 GB total storage. Past the limit, queries fail until 00:00 UTC. The Workers Free limits (requests per day, CPU time) came from a third-party page and are not verified (G-11).

## Consequences

- No monthly cost for the pilot. If the app outgrows the free tier, the paid plan or a move to PostgreSQL changes only the repository and the transport, because of the layers.
- Queries must stay light: index by store, item and date, and read only the rows a screen needs.
- A different stack from TDMS (NestJS and PostgreSQL). This is deliberate: this app has to run free. Rules in the shared package are written to be portable.
- D1 is SQLite, so migrations use SQLite SQL and there is no stored procedure layer.

## Alternatives considered

- **Google Sheets and Apps Script:** Leo's earlier stack, free, but slow with daily counts across many stores and not what he chose for this project.
- **PostgreSQL with NestJS as in TDMS:** needs a host with a cost.
- **A spreadsheet per store:** the current problem; no forecast and no shared rules.
- **Plain HTML first, convert to React later:** possible, because the layers keep the API client, services and rules, but every screen is rewritten. Dropped because Leo prefers React and the mockup is only a design reference.
