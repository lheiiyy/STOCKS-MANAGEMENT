# Store Stock Management Business Rules

Every rule has a stable `RULE-nnn`, a tag (A approved, P proposed, ? open; see [README](README.md)), its source, and the planned test. Defaults marked *(setting)* are held as settings (section 7). Open items are in [DECISION-GATE](DECISION-GATE.md).

## 1. Dates and shelf life

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-001 | PD is day 1. Day n of a batch is PD + (n - 1) | A | Leo | SHELF-001 |
| RULE-002 | Dough is usable from day 3 to day 7 and expired from day 8. Example: PD Nov 1 is usable Nov 3 to Nov 7 | A | Leo | SHELF-002 |
| RULE-003 | The shelf-life clock runs from PD, not from the delivery date | A | Leo | SHELF-003 |
| RULE-004 | Delivered stock may arrive on the PD day or one day after the PD | A | Leo | SHELF-004 |
| RULE-005 | Carbo has a shelf life of 4 days from PD (from the commissary). Palabok has 5 days | A | Leo, 2026-11-18 | SHELF-005 |
| RULE-006 | Carbo and palabok are usable from day 1 | A | Leo, 2026-11-18 | SHELF-006 |
| RULE-007 | Withdrawn. There are no safety days: planning uses the last usable day. The risk of a late delivery is handled by recording the real PD (RULE-008). Example: carbo, PD Nov 1, is planned through Nov 4 | Withdrawn (D015) | Leo, 2026-11-18 | |
| RULE-008 | PD is recorded for an item when its shelf life is at most 10 days *(setting)*; the flag can be switched per item | A | Leo, 2026-11-18 | SHELF-008 |
| RULE-009 | An item without PD tracking is managed by total stock and is not scheduled by batch | P | Architecture | SHELF-009 |
| RULE-010 | A received line for a PD-tracked item cannot be saved without a PD. The screen pre-selects the store's default delay; staff change it with one tap | P | Leo's request, 2026-11-18 | SHELF-010 |

## 2. Supply

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-020 | A store is either delivered or makes the item in store | A | Leo | SUPPLY-001 |
| RULE-021 | A delivered store has 1 to 3 supply slots a week; a slot is an order day plus a receive day; authorised users change them | A | Leo | SUPPLY-002 |
| RULE-022 | PD of a delivered batch is the receive day minus the delay (0 or 1 day) | A | Leo | SUPPLY-003 |
| RULE-023 | For in-store production, PD is the production day and there is no delay | P | Architecture | SUPPLY-004 |
| RULE-024 | Order cut-off time per slot | ? (G-07) | | |

## 3. Daily count and usage

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-030 | Beginning stock today is the ending count of the previous day | P | Store sheet | COUNT-001 |
| RULE-031 | Used = beginning + received - wasted - ending. The store sheet does the same: its total goods available subtracts wastage, and stock used = goods available - ending. The sheet also subtracts pull-outs to other stores and back-ups; v1 has no transfers, so a transfer out would show as use until transfers are added | P (G-04: answer found in the sheet; Leo to confirm) | Store sheet | COUNT-002 |
| RULE-032 | The ending count cannot exceed beginning + received - wasted | P | Mockup | COUNT-003 |
| RULE-033 | Counts are in pieces. Trays = pieces divided by pieces per tray (REQ-013). Orders are whole trays | A | Store sheet | COUNT-004 |

## 4. Forecast

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-040 | Expected use for a weekday = average use on that weekday over the last N weeks (N = 4 *(setting)*) x (1 + extra percent, default 10 *(setting)*) | P | Mockup, plan | FCST-001 |
| RULE-041 | Usable stock is used oldest first | P | Mockup | FCST-002 |
| RULE-042 | A batch covers the days from its first usable day until the day before the next batch becomes usable, and never past its last usable day (RULE-002, RULE-005) | P | Mockup | FCST-003 |
| RULE-043 | Suggested quantity = expected use in the coverage window - usable stock on hand for that window, rounded up to whole trays *(setting)* | P | Mockup | FCST-004 |
| RULE-044 | A shortage is flagged for any day with expected use that no usable batch covers | P | Mockup | FCST-005 |
| RULE-045 | Staff can change the suggested quantity. The change affects that plan line only and never changes the rule or the settings | A for the adjustment; P for the detail | Leo | FCST-006 |
| RULE-046 | Past plan lines do not change when a rule or setting changes later | P | TDMS D008 | FCST-007 |

## 5. Waste and reporting

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-050 | Waste reasons come from a list. The store sheet uses: cancelled by customer, expired, with molds, damage, trimmings or scrap, mechanical error, slice booth returned, customer complaint | P | Store sheet | WASTE-001 |
| RULE-051 | Early use = pieces taken from a batch before its first usable day, found from oldest-first consumption of recorded batches | P | Leo's problem statement | WASTE-002 |

## 6. Data integrity and access

| RULE | Statement | Tag | Source | Test |
| --- | --- | --- | --- | --- |
| RULE-060 | Every record has a server-generated prefixed ID, never typed, reused or changed | A | Leo's standard | PLAT-001 |
| RULE-061 | Every table has `created_at`, `created_by`, `updated_at`, `updated_by`, `is_active`, `row_version`. Every change writes an `audit_log` row in the same transaction | A | Leo's standard | PLAT-002 |
| RULE-062 | Records are deactivated, never deleted | A | Leo's standard | PLAT-003 |
| RULE-063 | Saving a record changed by someone else returns "changed by another user, reload" (`row_version`) | A | Leo's standard | PLAT-004 |
| RULE-064 | Every protected call checks session and role on the server | A | Leo's standard | PERM-001 |
| RULE-065 | Audit rows cannot be edited or deleted | P | TDMS RULE-024 | PLAT-005 |

## 7. Settings

| CFG | Setting | Default | Scope | Tag |
| --- | --- | --- | --- | --- |
| CFG-001 | Withdrawn (was safety days, see RULE-007) | | | Withdrawn |
| CFG-002 | PD-tracking threshold (shelf life in days) | 10 | global; per-item override | A |
| CFG-003 | Extra percent on expected use | 10 | global; per-item override | P |
| CFG-004 | History weeks for the average | 4 | global | P |
| CFG-005 | Round suggested quantity up to whole trays | on | per item | P |
| CFG-006 | Default delay between PD and receive day | 0 | per store and item | P |
| CFG-007 | First usable day and last usable day | dough 3 and 7; carbo 1 and 4; palabok 1 and 5 | per item | A |
