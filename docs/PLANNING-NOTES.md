# Store Stock Management — Approved Plans

Purpose of this file: single source of truth for decisions already agreed with Leo.
Read this first in any new session instead of re-asking. Only add items Leo has confirmed.

> Structured documentation set (bible, requirements, rules, decisions, ADRs, data model, architecture, permissions, gates) is in [README](README.md). This file keeps the confirmed facts and the workbook study. Where it disagrees with DECISIONS, DECISIONS wins (for example, safety days were dropped and the front end is React).

Status key: CONFIRMED = Leo said it / agreed. OPEN = still to ask. PROPOSAL = Claude's suggestion, not yet agreed.

## 1. Goal (CONFIRMED)
Web app that helps stores forecast the right amount of each item to order, timed with
expiration, to prevent spoilage and wastage. Calculation is automated but adjustable.

## 2. Delivery schedule (CONFIRMED)
- Each store has its own schedule: 1, 2, or 3 deliveries per week. Must be easy to change.
- Each delivery slot = one pair: order day -> receive day.
  - Example: order Tue -> receive Sat. Another store: order Tue -> receive Fri.
- Lead time is therefore NOT fixed; it is derived per slot.
- PROPOSAL: store these as editable per-store settings rows (no hardcoding). Forecast reads
  them to compute lead time and coverage window (receive day -> next receive day).

## 3. Dough rule (CONFIRMED)
- Count the production date (PD) as day 1.
- Dough is NOT usable on days 1-2. Usable from day 3 through day 7. Expired from day 8.
  - Example: PD Nov 1 -> usable Nov 3 to Nov 7.
- Delivery may be same day as PD (Nov 1) or 1 day late (Nov 2). The clock runs from PD, not
  from delivery date.
- PROPOSAL: track stock per batch (each with PD). "Received" and "usable today" are different
  numbers. Make "usable from day X / expires after day Y" a per-item setting so other items
  can use the same logic. Flag late-delivery risk.
- PROPOSAL: an order for dough must forecast demand for the later usable window, not the days
  right after delivery.

## 3b. Facts from the real sheet "AP MALATE FC P12W2.xlsx" (CONFIRMED, read from the file)
- Workbook is Figaro Coffee Systems daily inventory: one tab per weekday (Mon to Sun), ~800 items, categories incl. DOUGH, DAIRY, MEAT/COLD CUT, VEGETABLES.
- Daily columns per item: beginning inventory (whole + loose), commissary/warehouse delivery, pull-outs, direct purchases, transfers, back-ups, wastage, physical count (ending), stock used, variance.
  - Used = beginning + received - ending (checked: Mon medium dough 86 + 300 - 257 = 129). In the file's formulas, stock used = total goods available - ending, and goods available already subtracts wastage, pull-outs to other stores and back-ups. Variance = sales-based figure minus stock used.
- Other tabs: DOUGH PRODUCTION (date, time, trays per size, made by, validated by; empty this week), SPOILAGE, DATA - SPOILAGE, ITEM USAGE, RR - POR (receiving).
- Dough sizes: Medium 9 in = 12 pcs/tray, Family 12 in = 9 pcs/tray, Big family 14 in = 6 pcs/tray. Also thin-crust dough lines.
- P12W2 = Nov 9 to 15, 2026. Dough used per day (pcs), Mon..Sun:
  - Medium 129,114,119,109,154,178,133
  - Family 152,148,127,125,165,175,166
  - Big family 169,205,187,213,301,235,229
- Dough delivered that week (pcs): Medium Mon 300, Wed 84, Thu 84, Fri 84, Sat 240. Family 225,135,180,135,180. Big 300,180,240,300,300. No Tue or Sun delivery.
- The sheet does not record production dates for delivered dough (only on trays).

## 3d. Study of the other workbook tabs, AP Malate P12W2 (CONFIRMED, read from the file; one week only)
- Spoilage tab: date, item, qty, unit cost, total cost, checked by, reason. Reasons seen: CANCELLED BY CUSTOMER / GRAB, EXPIRED, DAMAGE (list also has WITH MOLDS, TRIMMINGS/SCRAP, MECHANICAL ERROR, SLICEBOOTH RETURNED, CUSTOMER COMPLAINT). Daily spoilage % of net sales is already tracked.
- Week total spoilage = PHP 3,568 on net sales of about PHP 1.38M (about 0.26%). By reason: cancelled by customer/Grab 2,228 (14 lines), expired 723 (10 lines), damage 617 (11 lines).
- Dough waste = 19 pcs, PHP 448 (about 12.6% of spoilage). Only 3 pcs were EXPIRED dough (1 big family, 2 family, about PHP 58). 16 pcs were DAMAGE, mostly thin-crust dough. Most expired value was sauces and cooked or marinated items (e.g. palabok sauce, chicken).
- Delivery pattern (FC SUMMARY): Delivery 1 commissary on Mon, Wed, Thu, Fri, Sat; Delivery 2 warehouse on Mon, Wed, Fri, Sat.
- ITEM USAGE tab maps products sold (by size, hand tossed and thin crust) to raw materials (dough, meat, sauces, dairy). Its dough use (e.g. Monday family 31, big family 66) is far below the counted dough use in the daily tab (family 152, big family 169). Cause not known; to be checked with Leo.
- RR - POR tab: pull-out receipts and receiving reports between stores (POR# and RR#). Stores do move stock to each other. Transfers are a possible later waste lever.
- Workbook is a shared template: FOR DROP DOWN lists stores; store code 102005 = Angels Malate. The "for checking" tab shows #NAME? and #N/A errors.
- Open question raised: expired dough is small at Malate, so ask Leo which stores or items have the real waste problem.

## 3e. Decisions from 18 Nov (CONFIRMED by Leo, except where marked proposal)
- Problem as Leo described it: dough is often used on day 1 or 2 because stock ran out or the forecast was wrong (this is invisible today because the sheet has no PD). Short shelf-life items also expire: carbo 4 days from PD (from the commissary), palabok 5 days.
- Day count: PD is day 1. The clock runs from PD, not from delivery. Items sometimes arrive 1 day after PD, so the store has fewer usable days.
- Per-item rules (not dough only): first usable day and last usable day. Values: dough day 3 to 7, carbo day 1 to 4, palabok day 1 to 5. Carbo and palabok usable from day 1: confirmed by Leo, 18 Nov.
- Safety days: dropped by Leo on 18 Nov (D015). Recording PD for short shelf-life items replaces the one-day deduction.
- Plan approval: Store Head and Area Manager both approve (D013). Carbo and palabok are in v1 (D014). Leo creates a new private repository; the name has not been received.
- PD recording: only for items with shelf life of 10 days or less (the 10 is a setting; each item can be switched on or off). Proposal for the Receiving screen: one tap for "PD: same day / 1 day before / 2 days before", extra line when a delivery has mixed PDs. Items over 10 days are tracked by delivery only.
- With real PD, the app can report dough used before day 3 and list stock about to expire.
- Study finding that supports this: at Malate, expired palabok sauce was the largest expired item in P12W2 (250 g, 960 g and 705 g on three days).

## 3c. Visual mockup (PROPOSAL, throwaway)
- Clickable mockup "Dough planner" published as a Claude artifact (private). Lifeline bars per batch (resting, usable, left over), editable delivery slots, usable-from/expires-after days, 0/1 day late arrival, safety buffer, tray rounding, +/- tray override per order.
- Forecast logic in the mockup: demand = last week's use by weekday x (1 + buffer); order qty = demand not covered by other usable stock inside that batch's coverage window; FIFO by oldest batch.
- Mockup sample values (not real): order days, on-shelf stock dates.

## 4. Open questions (OPEN)
- RESOLVED: usage comes from the daily count already on the store sheet (see 3b).
- Does Malate receive all dough (sheet shows daily commissary deliveries) or also make some? Which stores make their own (own commissary)? How often do they make it?
- Approach A (Google Sheets + Apps Script web app) proposed, not yet confirmed by Leo.
- Other items with aging or short use-by rules besides dough? Their day counts?
- Which items are wasted or run out most often (real example)?
- Who uses it (store staff, area managers, both) and on what device?
- Which brands/stores first (pilot)?
- Tech stack choice (not yet discussed).

## 5. Next steps
Finish brainstorming -> lay out approaches -> Leo picks -> written design doc -> Leo approves
-> implementation plan.
