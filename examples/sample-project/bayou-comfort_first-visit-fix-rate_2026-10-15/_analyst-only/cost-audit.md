# Cost Audit — Bayou Comfort · first-visit-fix-rate
Analyst-only. Estimates, not billing records. Prepared 2026-10-17.

| Item | Plan (expected) | Actual (est.) | Variance |
|---|---|---|---|
| Prompts run | 11 | 11 | 0% |
| Input tokens | 185k | 201k | +9% |
| Output tokens | 8k | 7.4k | −8% |
| Redos | 2 | 1 | −50% |
| Est. cost | — | — | — |

## METER scorecard
| Lever | Result | Evidence |
|---|---|---|
| Model tier | On plan | Default tier for all runs |
| Extract size | On plan | Two columns dropped before upload |
| Turns | Over | Stage 3 ran in one long chat; AV-01 redo re-read 40k tokens |
| Errors / redos | Under | One redo (AV-01) |
| Reply length | On plan | Bullets and tables throughout |

## Prompts to revise or retire
| Prompt | Issue | Recommendation |
|---|---|---|
| AV-01 | First pass returned all Pass with no numbers | Revise: require a number for every check |

## Next time, do these three things
1. Split Stage 3 into two chats (diagnose; verify and pressure test).
2. Add "show the number for each check" to AV-01.
3. Pre-aggregate visits by branch × job type × month before upload.
