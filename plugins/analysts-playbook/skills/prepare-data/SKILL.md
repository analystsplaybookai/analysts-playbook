---
name: prepare-data
description: >
  This skill should be used when an analyst needs to "check the data", "is the data
  complete", "did the AI use all the rows", "is this a fair comparison", "check for PII",
  "what data is missing", "mask sensitive fields", or wants to confirm data coverage,
  comparability, sensitivity, and repeatability before trusting results. Stage 2 (Prepare
  and govern) of the Analyst's Playbook.
metadata:
  version: "0.1.0"
---

# Stage 2: Prepare and Govern the Data

Confirm the data is safe to use, complete, comparable, and that numbers which must repeat are
produced by fixed calculations rather than AI narration.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout.
2. Read `../start-project/references/variables.md` for placeholder sources.
3. Load the project profile. If none exists, invoke start-project first and stop.
4. Read `references/prompts.md` for the exact prompt text, placeholders, and checks.

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
- **Owners.** Save DR-02's owner list (system, team, one question each) to the scorecard's
  "Next check / owner" column. These become the "who else to ask" step before recommending.

## Close the stage

1. Update `run_state.stages_completed` with "2 Prepare and govern".
2. Tell the analyst in 3–5 lines: guardrail decisions, coverage result, comparability
   caveats, and open data gaps with owners.
3. Recommend **Analyze and verify** next (verify-findings). Wait for go-ahead.
