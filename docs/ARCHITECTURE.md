# Store Stock Management Architecture

Status: DRAFT v0.1 for Leo's review. No code. Decision record: [ADR-0001](adr/0001-cloudflare-worker-d1-pwa.md). Tags are explained in [README](README.md).

## 1. Layers

```
Screens (PWA)
   │  calls only the API client
API client  (transport: mock | rest)
   │  HTTP /api/v1
Worker routes  (session, role check, validation, response envelope)
   │
Services  (business flow, one writing service per record type)
   │
Pure rules  (packages/shared: no I/O, no clock, no database)   Repository  (the only code that touches D1)
                                                                  │
                                                                 D1
```

| Layer | May call | Must not call |
| --- | --- | --- |
| Screens | API client | the Worker, D1, any rule |
| API client | HTTP (rest) or fixtures (mock) | business logic |
| Routes | services | repository, D1 |
| Services | pure rules, repositories, other services' public functions | another service's repository, D1 |
| Pure rules | other pure rules | anything with I/O, `Date.now`, global state |
| Repository | D1 | services, rules |

A layer calls only the layers below it. The clock and the ID generator are passed in as arguments so rules stay testable. Layer checks run in CI (proposed: dependency-cruiser, as in TDMS).

## 2. Modules

| Module | Owns tables | Main functions |
| --- | --- | --- |
| Items and rules | items, item_rules, lookups, settings | list items, change a rule (new version), change a setting |
| Stores and supply | areas, stores, supply_slots | edit a store's slots, set supply mode |
| Receiving | batches | record a received or produced batch with PD |
| Daily count | daily_counts | open the day (beginning from yesterday), save count, validate |
| Forecast | plan_lines | compute the plan on read, approve or adjust a line |
| Waste | waste_events | record waste with a reason, report |
| Users and audit | users, audit_log | sign in, role check, write audit rows |

## 3. Request flow: the Today screen

1. The screen asks the API client for the plan of one store and date.
2. The route checks the session and that the role may view this store.
3. The forecast service reads the store, its slots, the item rules, the last N weeks of counts, the open batches and today's approved lines.
4. The pure rules compute expected use, the batches in oldest-first order, the coverage windows, the suggested quantity and any shortage (RULE-040 to RULE-044).
5. The response carries each line with its reason ("why this number") and the 7-day covered or short strip.
6. Approving a line writes a `plan_lines` row and an `audit_log` row in one transaction.

## 4. What is stored and what is computed

Stored: items, rules, stores, slots, counts, batches, waste, approved plan lines, users, audit. Computed on read: expected use, remaining stock per batch, coverage, suggested quantity, shortage, early-use report. Computed values are not stored, so a rule change never leaves stale numbers (REQ-044).

## 5. Environments

| Environment | Database | Data |
| --- | --- | --- |
| Local | `wrangler dev` with a local D1 | fixtures |
| Test | a separate D1 database | dummy data only |
| Live | the live D1 database | real data after Leo's approval |

Browser tests run on localhost with the mock transport, so no sign-in is needed. Backup and restore of the live D1 database is not yet verified (G-11).

## 6. Repository layout (proposed)

```
STOCKS-MANAGEMENT/           (private)
├── CLAUDE.md
├── README.md
├── docs/                    (this set)
├── db/migrations/           (plain SQL, numbered)
├── apps/worker/src/         (routes, services, repository)
├── apps/web/                (React + Vite PWA: screens, api client)
├── packages/shared/         (pure rules and types)
├── tests/                   (unit, fixtures, e2e)
├── .github/workflows/       (ci.yml, deploy.yml)
└── wrangler.toml
```

The front end is React with Vite (D017). TypeScript is proposed for the Worker, the web app and the shared package, as in TDMS (G-05).

## 7. Limits

D1 free plan: 5 million rows read a day, 100,000 rows written a day, 5 GB total (verified on Cloudflare's page). Workers Free request and CPU limits are not verified (G-11). Design rules that follow: index by (store, item, date); read only what a screen needs; page lists; never scan the whole counts table for one store.

## 8. Security

- Sign-in by employee number and password (proposed, G-08); passwords hashed with a modern algorithm supported on Workers.
- Session and role checked on the server for every call (RULE-064).
- Secrets only in GitHub and Cloudflare secrets. No tokens, IDs or `.env` files in git.
- Private repository (REQ-063).

## 9. Growth path

If the free tier is outgrown, replace the repository and the REST transport (for example D1 to PostgreSQL). Screens, services and rules stay unchanged.
