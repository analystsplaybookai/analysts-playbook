# METER: Cost Levers, Estimates, and Templates

## The five levers

| Lever | What drives cost | Typical savings move |
|---|---|---|
| **M**odel tier | Larger models cost several times more per token than smaller ones. | Use a smaller tier for formatting, summarizing, and logging steps; keep the larger tier for diagnosis and pressure testing. |
| **E**xtract size | Every row and column sent to AI is paid for, often more than once across turns. | Send only the columns and period the question needs; aggregate before sending; compute fixed figures with code. |
| **T**urns | Each turn re-reads the conversation so far, so long chats compound cost. | Start a fresh chat per stage with the project instructions file instead of one endless thread. |
| **E**rrors / redos | Vague prompts and red-flag outputs lead to retries. | Fill every placeholder; use the specific challenge (AV-06) instead of "are you sure?"; stop after 2 redos and rethink. |
| **R**eply length | Output tokens usually cost more than input tokens. | Ask for bullets and tables; set a length in the output style. |

## Estimating rules

- Tokens ≈ characters ÷ 4 (English text and CSV). Round to the nearest thousand.
- Data loaded once in a chat is re-read on later turns. Estimate input per prompt as:
  instructions (~1.5k) + data extract + conversation so far.
- Typical reply sizes: short check 300–600 tokens; table or scorecard 600–1,200;
  brief or scenarios 800–1,500.
- Redo allowance for the plan: Core depth 15% of prompts, Extended 25%.
- Low / expected / high = expected × 0.6 / 1.0 / 1.8.

## cost-plan.md template

```markdown
# Cost Plan — {{organization}} · {{project_slug}}
Analyst-only. Estimates, not billing records. Prepared {{date}}.

| Item | Value |
|---|---|
| Depth | Core / Extended ({{n}} prompts) |
| Model tier | {{model_tier}} |
| Rates used | ${{in}} / ${{out}} per million tokens — source: {{source}}, {{date}} |
| Data extract | {{files}} ≈ {{tokens}} tokens |

## Estimate
| Scenario | Input tokens | Output tokens | Est. cost |
|---|---|---|---|
| Low | | | |
| Expected | | | |
| High | | | |

## METER outlook
| Lever | Expectation | Risk |
|---|---|---|
| Model tier | | |
| Extract size | | |
| Turns | | |
| Errors / redos | | |
| Reply length | | |

## Savings moves before starting
1.
2.
3.
```

## cost-audit.md template

```markdown
# Cost Audit — {{organization}} · {{project_slug}}
Analyst-only. Estimates, not billing records. Prepared {{date}}.

| Item | Plan (expected) | Actual (est.) | Variance |
|---|---|---|---|
| Prompts run | | | |
| Input tokens | | | |
| Output tokens | | | |
| Redos | | | |
| Est. cost | | | |

## METER scorecard
| Lever | Result | Evidence |
|---|---|---|
| Model tier | On plan / Over / Under | |
| Extract size | | |
| Turns | | |
| Errors / redos | | |
| Reply length | | |

## Prompts to revise or retire
| Prompt | Issue | Recommendation |
|---|---|---|

## Next time, do these three things
1.
2.
3.
```
