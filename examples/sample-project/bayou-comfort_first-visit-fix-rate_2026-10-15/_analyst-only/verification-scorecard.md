# Verification Scorecard

| # | Finding (metric · segment · period) | Evidence tag | Records | Checks run | Status | Next check / owner |
|---|---|---|---|---|---|---|
| 1 | First-visit fix rate fell from 81% to 73%, Q3 2025 → Q3 2026, all branches | Confirmed | 4,210 | INT-01, DR-01, AV-01 | Share now | — |
| 2 | 70% of the drop is heat-pump jobs where the needed part was not on the truck (Branches 2 and 3) | Confirmed | 612 | AV-11, AV-01, AV-03 | Share now | — |
| 3 | Newer technicians drove the drop | Speculative | 4,173 | AV-01, AV-03 | Hold | Tenure split shows similar rates; training records not in data — HR |
| 4 | Supplier delays caused the missing parts | Speculative | — | AV-07 | Share as hypothesis | Purchasing: did heat-pump part lead times change in Q3? |

## Guardrail decisions

| Date | Field / data | Rule | Decision |
|---|---|---|---|
| 2026-10-15 | Customer address column | Internal policy | Dropped before upload; not needed for the question |
