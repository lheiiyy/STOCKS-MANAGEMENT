# Store Stock Management Logical Data Model v0.1

Status: **DRAFT for Leo's review.** Logical only: no SQL, no migration written. Rules: [BUSINESS-RULES](BUSINESS-RULES.md). Architecture: [ARCHITECTURE](ARCHITECTURE.md).

## 1. Principles

| # | Principle | Source |
| --- | --- | --- |
| P1 | IDs are server-generated, prefixed, never reused, never typed | RULE-060 |
| P2 | Links use IDs; names are display values only | Leo's standard |
| P3 | Records that were used by other records are deactivated, never deleted | RULE-062 |
| P4 | Each record type has one writing service | ARCHITECTURE |
| P5 | Computed values (remaining stock, coverage, suggestions) are derived, not stored | REQ-044 |
| P6 | Dates are `yyyy-mm-dd` in the business time zone; timestamps are ISO 8601 UTC. The zone is open (G-12) | |
| P7 | Counts are whole pieces | RULE-033 |

### Record profiles

| Profile | Columns | Concurrency |
| --- | --- | --- |
| **MUT** mutable | `id`, `created_at`, `created_by`, `updated_at`, `updated_by`, `is_active`, `row_version` | update carries `row_version`; mismatch returns a conflict |
| **APP** append-only | `id`, `created_at`, `created_by` | none; rows are never edited |
| **VER** versioned | APP columns + `effective_from`, `supersedes_id` | insert only; current = latest with `effective_from` not after the date |

## 2. Tables

| Table | Prefix | Profile | Key columns | Links |
| --- | --- | --- | --- | --- |
| areas | ARE | MUT | name | |
| stores | STO | MUT | code (text, for example 102005), name, supply_mode (`delivered` or `own`), default_delay_days (0 or 1) | area_id |
| supply_slots | SLT | MUT | kind (`delivery` or `production`), order_dow (null for production), receive_dow or production_dow, cutoff_time (open, G-07) | store_id, item_id (null = all items) |
| items | ITM | MUT | code, name, size_label, uom, pieces_per_tray, category | |
| item_rules | RUL | VER | first_usable_day, last_usable_day, track_pd, extra_pct (null = global) | item_id |
| daily_counts | CNT | MUT | count_date, beginning_pcs, received_pcs, wasted_pcs, ending_pcs, status (`draft` or `submitted`) | store_id, item_id |
| batches | BAT | MUT | pd_date (null if PD is not tracked), arrive_date, qty_pcs, source (`delivery` or `made`) | store_id, item_id, slot_id |
| plan_lines | PLN | MUT | for_date, suggested_qty_trays (snapshot), approved_qty_trays, status (`draft` or `approved`), adjust_reason | store_id, item_id |
| waste_events | WST | APP | event_date, qty_pcs, reason_code | store_id, item_id, batch_id (nullable) |
| users | USR | MUT | employee_no (text), name, role, password_hash | store_id or area_id (scope) |
| lookups | LKP | MUT | group (for example `waste_reason`), code, label, sort | |
| settings | CFG | MUT | key, value, scope (global, item, store) | item_id, store_id (nullable) |
| audit_log | AUD | APP | entity, record_id, action, before (text), after (text), at | user_id |

## 3. Relationships

- A store has many supply slots, counts, batches, plan lines and waste events.
- An item has many versions of its rule. The current rule is the latest version whose `effective_from` is not after the date used (a plan uses the rule in force on the day it computes).
- A batch belongs to one store and one item and may point to the slot that delivered it.
- Remaining stock per batch is derived: pieces received minus pieces consumed oldest first, using the daily counts.

## 4. Indexes (proposed)

`daily_counts (store_id, item_id, count_date)` unique; `batches (store_id, item_id, pd_date)`; `plan_lines (store_id, for_date)`; `waste_events (store_id, event_date)`; `audit_log (entity, record_id)`.

## 5. Open items

Per-store overrides of an item's rule (not modelled); the business time zone (G-12); whether a second batch of the same PD on one day is one row or two (proposal: two rows, one per delivery).
