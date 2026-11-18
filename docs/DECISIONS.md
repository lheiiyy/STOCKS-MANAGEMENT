# Store Stock Management Decisions (approved only)

Approved by Leo in the planning chat. Open questions are not here: see [DECISION-GATE](DECISION-GATE.md). Long form of the larger decisions is in [adr/](adr/). Dates before 2026-11-18 are recorded as "Nov 2026" because the chat did not date them.

| ID | Topic | Notes | Date |
| --- | --- | --- | --- |
| D001 | Start with dough only | Extended by D009 to other short shelf-life items through item rules | Nov 2026 |
| D002 | Users: Store Staff, Store Head, Area Manager | Admin is proposed, not approved | Nov 2026 |
| D003 | Hosting: free-tier full stack on Leo's Cloudflare account (Worker and D1), as in the Liquidation form app | ADR-0001 | Nov 2026 |
| D004 | First screen: what to order or make today, per store | | Nov 2026 |
| D005 | Delivery schedule per store is easy to change: 1 to 3 slots a week, each with an order day and a receive day. Stores that make their own dough have production days | | Nov 2026 |
| D006 | Dough rule: PD is day 1, usable day 3 to day 7, may arrive on the PD day or one day after | | Nov 2026 |
| D007 | Approved plans are kept as markdown in a repository to save tokens | | Nov 2026 |
| D008 | The calculation is automatic and adjustable by the store | | Nov 2026 |
| D009 | Item rules are data: first usable day and last usable day. This covers dough, carbo (4 days from PD) and palabok (5 days). Safety days were dropped, see D015 | ADR-0002 | 2026-11-18 |
| D010 | Safety days default to 1 per item, to use stock before it expires. Superseded by D015 | ADR-0002 | 2026-11-18 |
| D011 | PD is recorded only for items with shelf life of 10 days or less; the threshold is a setting and each item can be switched | ADR-0002 | 2026-11-18 |
| D012 | These documents follow the TDMS structure (README, PROJECT-BIBLE, REQUIREMENTS, BUSINESS-RULES, DECISIONS with ADR, DECISION-GATE and the rest) | | 2026-11-18 |
| D013 | Plan approval: Store Head and Area Manager both approve (G-01). Whether Store Staff may adjust or approve was not answered and stays denied | PERMISSIONS | 2026-11-18 |
| D014 | Carbo and palabok are in v1 and are usable from day 1 (G-02). Read from Leo's single YES to a two-part question | | 2026-11-18 |
| D015 | No safety days. PD is recorded for items with short shelf life instead (G-03). Supersedes D010 | ADR-0002 | 2026-11-18 |
| D016 | The project has its own private repository, created by Leo: `lheiiyy/STOCKS-MANAGEMENT` | | 2026-11-18 |
| D017 | The front end is React (with Vite). Leo asked whether plain HTML could be converted to React later: it can, but every screen is rewritten, so React starts now | ADR-0001 | 2026-11-18 |
