# ADR-0002 · Item rules and production-date tracking

- **Status:** Accepted by Leo on 2026-11-18.
- **Decider:** Leo. **Date:** 2026-11-18. **Decision log entries:** D009, D011, D014, D015. D010 (safety days) was proposed and superseded the same day.

## Context

The first design covered dough only, with one rule (usable day 3 to day 7). Leo then described two failures:

1. Dough is often used on day 1 or day 2 when stock runs out or the forecast is wrong. This is invisible because the sheet has no PD.
2. Items with a short shelf life expire. Carbo is 4 days from PD (from the commissary) and palabok is 5 days. Delivered items sometimes arrive one day after PD, which shortens the days the store can use them.

The store workbook for one week agrees: palabok sauce was the largest expired item.

## Decision

1. Shelf-life rules are rows of data per item, not code: first usable day and last usable day. Dough is day 3 to day 7. Carbo is day 1 to day 4. Palabok is day 1 to day 5. Dough, carbo and palabok use the same calculation.
2. The clock runs from PD. PD is day 1. The delivery date is not the start.
3. PD is recorded only for items whose shelf life is at most 10 days. The threshold is a setting and each item can be switched on or off. Receiving shows one tap for the PD of a tracked item, with the store's default pre-selected.
4. There are no safety days. A one-day deduction from shelf life was proposed so that stock is used before it expires. Leo dropped it because recording PD for short shelf-life items does the same job with real dates.

## Consequences

- Adding an item means adding a row and its rule. No release is needed.
- With recorded PD the forecast uses the real age of each batch, so a late delivery is already counted. The app can also report dough used before day 3, which the sheet cannot show.
- A late delivery costs a store one usable day. Example: carbo, PD Nov 1, arriving Nov 2, is usable Nov 2 to Nov 4.
- Staff do one extra tap per tracked delivery line.
- Items over 10 days are managed by total stock (RULE-009, proposed).
- If expiry still happens in practice, a safety margin can be added back as a setting without changing the design.

## Alternatives considered

- **PD for every item:** the most exact, but heavy for staff on items that last weeks.
- **No PD, a flat one-day deduction:** the first idea in the chat. Simple, but the early use of dough cannot be measured and a same-day delivery loses a day for nothing.
- **Dough-only rules:** does not help carbo and palabok.
