---
name: prepare-data
description: >
  This skill should be used when an analyst needs to "check the data", "is the data
  complete", "did the AI use all the rows", "is this a fair comparison", "check for PII",
  "what data is missing", "mask sensitive fields", or wants to confirm data coverage,
  comparability, sensitivity, and repeatability before trusting results. Stage 2 (Prepare
  and govern) of the Analyst's Playbook.
metadata:
  version: "0.3.0"
---

# Stage 2: Prepare and Govern the Data

Confirm the data is safe to use, complete, comparable, and that numbers which must repeat are
produced by fixed calculations rather than AI narration.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout.
2. Read `../start-project/references/variables.md` for placeholder sources.
3. Load the project profile. If none exists, invoke start-project first and stop.
4. Read `references/prompts.md` for the exact prompt text, placeholders, and checks.
5. Check the profile's `risk.final_tier` and apply its controls (conventions §10). If the
   scope, data source, or audience has changed since it was scored, rescore first.

## Run order

| Order | Prompt | Depth | Run when |
|---|---|---|---|
| 1 | DR-04 Sensitivity, cost and repeatability check | Extended — **but always run when the profile lists any regulatory rule** | Before any new data is loaded or shared with AI, and before a result is reused in a recurring report. |
| 2 | DR-01 Data coverage check | Core | After any AI summary of text, reviews, surveys, or large files. |
| 3 | DR-03 Like-for-like comparison check | Extended | The question compares groups, channels, regions, or periods. |
| 4 | DR-02 Missing-data and owner inventory | Extended | A finding needs a cause the dataset cannot show. |

## Stage-specific rules

- **Governance first.** DR-04 is the gate. If it flags personal, confidential, or regulated
  fields, stop and get the analyst's decision (mask, aggregate, drop) before any other
  prompt reads the data. Record the decision in the scorecard's Guardrail table.
- **Fixed calculations.** From DR-04's repeatability split, list which figures must come from
  a formula, SQL, or script. Where code execution is available, compute those figures with
  code and cite the code output, not AI estimation. Note this list in the scorecard.
- **Smaller extract.** If DR-04 suggests a smaller extract, tell the cost-and-log skill
  (update the profile's `settings.extract_note`) so the cost plan reflects it.
- **Coverage.** If DR-01 shows used < total without a stated reason, rerun on the full set
  and record what changed. Count it as a redo.
- **Data-quality log.** Record every data problem found in this stage (gaps in dates or
  locations, duplicates removed, blank or invalid fields, late-arriving data) in
  `_analyst-only/data-quality-log.md`, with its size and how it was handled. The brief
  must account for every item on this log, so log it even if it looks minor.
- **One cleaned extract.** Once cleaning is decided, save the cleaned data (or the code that
  produces it) once and take every count and rate in later stages from it. Record the
  cleaned record count in the data-quality log. Never quote a raw count next to a cleaned one.
- **Paired and timing checks.** When DR-03 involves the same units in both groups, calculate
  the comparison per unit and then average it; pooling rows does not count as paired. Check
  whether exposure timing lines up with something else (stage of a customer's life cycle,
  calendar month, a price or policy change).
- **Owners.** Save DR-02's owner list (system, team, one question each) to the scorecard's
  "Next check / owner" column. These become the "who else to ask" step before recommending.

## Close the stage

1. Update `run_state.stages_completed` with "2 Prepare and govern".
2. Tell the analyst in 3–5 lines: guardrail decisions, coverage result, comparability
   caveats, and open data gaps with owners.
3. Recommend **Analyze and verify** next (verify-findings). Wait for go-ahead.
