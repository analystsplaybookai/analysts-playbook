---
name: cost-and-log
description: >
  This skill should be used when an analyst asks "how much will this cost", "plan the AI
  cost", "estimate tokens", "log this prompt", "cost audit", "what did this project cost",
  "METER check", "why was this expensive", or when another Playbook skill needs a cost plan
  before analysis or a cost audit at close. Keeps every cost and log file in the
  analyst-only folder. Uses the METER method (Model tier, Extract size, Turns,
  Errors/redos, Reply length).
metadata:
  version: "0.2.0"
---

# Cost Plan, Run Log, and Cost Audit

Plan AI usage cost before the work, log every run during it, and audit cost after it, using
the five METER levers. All output stays in `_analyst-only/`. Never write costs to
`01_deliverables/`.

Read `../start-project/references/conventions.md` first, then `references/meter.md` for the
levers, estimating rules, and file templates.

## Modes

Pick the mode from the request or the calling skill:

| Mode | Trigger | Output |
|---|---|---|
| plan | start-project Step 6, or "estimate / plan the cost" | `_analyst-only/cost-plan.md` |
| log | "log this", or any manual prompt run outside a stage skill | new row in `_analyst-only/prompt-log.csv` |
| audit | deliver-brief close, or "what did this cost / cost audit" | `_analyst-only/cost-audit.md` |

## Plan mode

1. Read the profile: depth, data inventory, model tier, extract note.
2. Count the prompts that will run at this depth (stage tables in each stage skill).
   Core: 11 always (SET-01, SET-02, INT-01, INT-03, DR-01, AV-01, AV-02, AV-03, AV-07,
   AV-11, CO-01), plus up to 3 conditional (DR-04 when regulated, AV-14 when 4+ findings,
   CO-04 when the action is costly) and AV-06 on demand. Extended: all 29.
3. Estimate each METER lever using `references/meter.md`. Estimate data size from the actual
   files if they are in the folder (characters ÷ 4 ≈ tokens).
4. Get rates: use `settings.rates` from the profile if set. If not, look up the current
   published per-million-token input and output prices for the model tier in use (web search
   when available), record price, source, and date in the plan, and save them to
   `settings.rates`. If no price can be confirmed, report tokens only and leave dollars blank.
   Never invent a price.
5. Write `cost-plan.md` from the template in `references/meter.md`: low / expected / high
   estimate, the lever that drives most of the cost, and up to three specific savings moves.
6. Return a one-line summary to the caller (e.g., "Expected ~180k tokens (~$X); biggest lever:
   Extract size — a 3-column aggregate would cut it by about half").

## Log mode

Append one row to `prompt-log.csv` with every column filled (see conventions §4). Use the
prompt ID if one applies; for ad hoc prompts use `ADHOC-<n>` and a short name.

## Audit mode

1. Read `prompt-log.csv` and `cost-plan.md`.
2. Total tokens and estimated cost; compare with the plan (variance %).
3. Score each METER lever as On plan / Over / Under with one sentence of evidence
   (e.g., "Errors/redos: Over — 5 redos, 3 of them on AV-08").
4. Flag prompts to Revise or Retire: any prompt with 2+ redos or a red flag on most runs.
   Write the recommendation into the log's Keep / Revise / Retire column.
5. Write `cost-audit.md` from the template in `references/meter.md`, ending with the three
   changes that would cut cost most on the next run of this type of project.
6. Return a one-line summary to the caller.

## Rules

- Label every figure as an estimate. These are not billing records. If the analyst has
  actual usage figures from their account or API console, ask for them and record both.
- Keep model names, rates, and token counts out of anything client-facing.
