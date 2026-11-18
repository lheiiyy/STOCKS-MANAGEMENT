# Store Stock Management Decision Gate v0.3 (2026-11-18)

Open questions that block or shape a build. Answers are recorded here and then moved into [DECISIONS](DECISIONS.md). Nothing below is guessed in the build: a feature that depends on an open gate stays out or is denied until Leo answers.

## A. Open

| Gate | Question | Why it matters | Proposal |
| --- | --- | --- | --- |
| G-01 (rest) | Can Store Staff adjust a suggested quantity or approve a plan? Who edits item rules and settings? | Permissions | Staff adjust, do not approve; Admin edits rules (P) |
| G-04 | Usage with wastage: confirm. The sheet's total goods available already subtracts wastage, and stock used = goods available - ending, so the proposal matches the sheet | RULE-031 | Use beginning + received - wasted - ending; add transfers out later |
| G-05 (rest) | Language: TypeScript for the Worker, the web app and the shared package, as in TDMS | ARCHITECTURE section 6 | TypeScript |
| G-06 | Which stores make their own dough, and how often? Which store is the pilot? | Supply slots, pilot | AP Malate as the pilot |
| G-07 | Order cut-off times, and the default delay (0 or 1 day) per store | RULE-024, CFG-006 | Default 0, set per store |
| G-08 | Sign-in method | Permissions, security | Employee number and password |
| G-09 | Waste target (spoilage as a percent of net sales) and which waste matters most | Success measure | Baseline 0.26% at Malate, no target yet |
| G-10 | The workbook's item-usage tab shows far less dough than the counted use (family dough 31 against 152 on one Monday). The sheet also has a variance column (sales-based figure minus stock used). What does the item-usage figure represent? | Whether usage comes from counts or sales | Use counts until explained |
| G-11 | Verify Workers Free limits (requests, CPU) and how to back up and restore the live D1 database on the free plan | Risk, operations | Check Cloudflare's documentation before the pilot |
| G-12 | Business time zone for dates | Date rules | Asia/Manila |

## B. Answered

| Gate | Answer | Recorded as |
| --- | --- | --- |
| Hosting | Free-tier full stack on Leo's Cloudflare account | D003 |
| First scope | Dough first | D001 |
| Users | Store Staff, Store Head, Area Manager | D002 |
| First screen | What to order or make today, per store | D004 |
| Short shelf-life items | Item rules; PD recorded for items of 10 days or less | D009, D011 |
| G-01 (approval) | Store Head and Area Manager both approve | D013 |
| G-02 | Yes: carbo and palabok are in v1 and usable from day 1 (read from one YES to a two-part question) | D014 |
| G-03 | No safety days for any item; PD recording replaces them | D015 |
| G-05 (repository and front end) | Repository `lheiiyy/STOCKS-MANAGEMENT` (private); React for the front end | D016, D017 |
