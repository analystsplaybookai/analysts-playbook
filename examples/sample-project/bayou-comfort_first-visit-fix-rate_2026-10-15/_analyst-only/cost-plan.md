# Cost Plan — Bayou Comfort · first-visit-fix-rate
Analyst-only. Estimates, not billing records. Prepared 2026-10-15.

| Item | Value |
|---|---|
| Depth | Core (11 prompts) |
| Model tier | default |
| Rates used | Not set — sample reports tokens only |
| Data extract | service_calls_2025_2026.csv, 6 columns ≈ 14k tokens |

## Estimate
| Scenario | Input tokens | Output tokens | Est. cost |
|---|---|---|---|
| Low | 110k | 5k | — |
| Expected | 185k | 8k | — |
| High | 330k | 14k | — |

## METER outlook
| Lever | Expectation | Risk |
|---|---|---|
| Model tier | Default tier throughout | Low |
| Extract size | 14k tokens re-read each turn | Medium — largest driver |
| Turns | One chat per stage | Low |
| Errors / redos | 15% allowance (~2 redos) | Medium |
| Reply length | Bullets and tables | Low |

## Savings moves before starting
1. Drop the customer address and notes columns (also required by policy).
2. Start a new chat per stage using project-instructions.md.
3. Compute the fix-rate figures with a formula, not AI narration.
