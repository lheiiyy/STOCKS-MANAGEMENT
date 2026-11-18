# Store Stock Management Permissions v0.1

Status: **DRAFT for Leo's review.** Roles are approved (D002); the Admin role and every cell marked P or ? are not. A cell marked **?** is built as **deny** until Leo decides. Tags are explained in [README](README.md).

Each user has one role and a scope: Store Staff and Store Head see their own store; Area Manager sees the stores of one area; Admin sees all.

| Action | Store Staff | Store Head | Area Manager | Admin |
| --- | --- | --- | --- | --- |
| View the Today plan and the 7-day strip | yes | yes | yes (area) | yes |
| Enter count, receiving and waste | yes | yes | no (P) | yes |
| Adjust a suggested quantity | ? | yes (P) | no (P) | yes |
| Approve the plan | ? | yes | yes | yes |
| Edit a store's supply slots | no | yes, own store (P) | yes, own area (P) | yes |
| Edit item rules and settings | no | no | ? | yes |
| Manage stores and users | no | no | no | yes |
| View reports | own store | own store | area | all |
| View the audit log | no | no | no | yes |

Rules for every row: the server checks session, role and scope on each call (RULE-064). A change by Admin to another user's record carries a reason in the audit log (proposed, as in TDMS). Approval by Store Head and Area Manager is approved (D013). The `?` cells are still open: whether Store Staff may adjust or approve a plan, and who edits item rules (G-01), and the sign-in method (G-08).
