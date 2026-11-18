# Store Stock Management Requirements

Tags are explained in [README](README.md). Rule-level detail: [BUSINESS-RULES](BUSINESS-RULES.md). Open items: [DECISION-GATE](DECISION-GATE.md).

## Platform and architecture

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-001 | Free-tier full stack on Leo's own Cloudflare account: a Worker and a D1 database | A | Leo, hosting answer; ADR-0001 |
| REQ-002 | Installable PWA that works on phone, tablet and desktop; compact so more fits in one view | A | Leo, mockup requests |
| REQ-003 | Layers: screens, API client, services, repository, database; the API client has a `mock` and a `rest` transport | A | Leo's standard |
| REQ-004 | Deployed from GitHub Actions with the Cloudflare token and account ID as repository secrets | A | Leo, like the Liquidation app |
| REQ-005 | Approved plans are kept as markdown files in a repository | A | Leo |
| REQ-006 | The front end is React (with Vite) | A | Leo, 2026-11-18 (D017) |

## Items and rules

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-010 | Each item has rules held as data: first usable day and last usable day | A | Leo, 2026-11-18 |
| REQ-011 | Shelf life is counted from PD, and PD is day 1 | A | Leo |
| REQ-012 | PD is recorded only for items whose shelf life is at or below a threshold (default 10 days); each item can be switched on or off | A | Leo, 2026-11-18 |
| REQ-013 | Dough sizes: Medium 9 in (12 pcs per tray), Family 12 in (9), Big family 14 in (6) | A | Store sheet |
| REQ-014 | Withdrawn: safety days. Leo dropped them because PD is recorded for items with short shelf life | Withdrawn | Leo, 2026-11-18 (D015) |

## Stores and supply

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-020 | A delivered store has 1 to 3 supply slots a week, each with an order day and a receive day; they are easy to change | A | Leo |
| REQ-021 | The gap between order and receive differs by slot (for example order Tuesday, receive Friday or Saturday) | A | Leo |
| REQ-022 | A store that makes its own dough has production days instead of delivery | A | Leo |
| REQ-023 | Delivered stock may arrive on the PD day or one day after | A | Leo |

## Daily operations

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-030 | Daily count per store and item: beginning, received, wasted, ending; used is calculated | P (G-04) | Store sheet |
| REQ-031 | Beginning stock is yesterday's ending count, filled in automatically | P | Store sheet |
| REQ-032 | The ending count cannot exceed the stock available; the screen shows why | P | Mockup |
| REQ-033 | Receiving records the quantity per item, and the PD for items where PD is tracked, with one tap | A | Leo, 2026-11-18 |
| REQ-034 | Waste is recorded with a reason from a list (expired, damage, and the others the store already uses) | P | Store sheet |

## Forecast

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-040 | The first screen shows what to order or make today, per store | A | Leo |
| REQ-041 | The calculation is automatic and adjustable; the store sees why a number was suggested | A | Leo |
| REQ-042 | The screen shows the next 7 days as covered or short | P | Mockup |
| REQ-043 | Stock is planned to be used oldest first | P | Mockup |
| REQ-044 | The forecast is computed on read; only approved quantities are stored | P | Architecture |

## Reporting

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-050 | Report expired, damaged and early-use pieces by item, store and period | P | Workbook study |
| REQ-051 | Dough used before its first usable day is measured from recorded PD | P | Leo's problem statement |

## Security and data integrity

| ID | Requirement | Tag | Source |
| --- | --- | --- | --- |
| REQ-060 | Roles: Store Staff, Store Head, Area Manager; Admin added | A for the first three; P for Admin | Leo |
| REQ-061 | Every call checks session and role on the server | A | Leo's standard |
| REQ-062 | Prefixed server-generated IDs; audit columns on every table; `audit_log`; soft delete only | A | Leo's standard |
| REQ-063 | Private repository | A | Leo's standard |
| REQ-064 | Surgical patches; structural or calculation changes need Leo's approval first | A | Leo's standard |
