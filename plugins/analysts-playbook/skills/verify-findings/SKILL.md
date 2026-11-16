---
name: verify-findings
description: >
  This skill should be used when an analyst wants to "verify this finding", "pressure test
  the analysis", "stress test my conclusion", "what am I missing", "are you sure",
  "diagnose what's driving the change", "project scenarios", "recommend actions", or
  "which findings can I share". Covers diagnosis, assumption checks, alternate
  explanations, and confidence triage. Stage 3 (Analyze and verify) of the Analyst's
  Playbook.
metadata:
  version: "0.1.0"
---

# Stage 3: Analyze and Verify

Move from a bare fact to a defensible explanation, test every finding before it is shared,
and sort findings into share now / share as hypothesis / hold.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout.
2. Read `../start-project/references/variables.md` for placeholder sources.
3. Load the project profile and `_analyst-only/verification-scorecard.md`. If no profile
   exists, invoke start-project first and stop.
4. Read `references/prompts.md` for the exact prompt text, placeholders, and checks.

## Run order

The stage has four passes. Run each pass on every Open finding in the scorecard.

| Pass | Prompt | Depth | Purpose |
|---|---|---|---|
| A. Diagnose | AV-11 Climb the ladder: diagnose | Core | Move "X changed" to segment-level drivers. |
| B. Verify | AV-02 Surface hidden assumptions | Core | Always first after any analysis. |
| | AV-01 Six-point reliability sweep | Core | Scorecard per finding. |
| | AV-03 Evidence label per finding | Core | Separate shown from inferred. |
| | AV-04 Wider-window durability test | Extended | Finding comes from a short window. |
| | AV-05 Targeted assumption test | Extended | A specific cause is blamed. |
| | AV-10 Sequence and lag check | Extended | A cause is suspected; confirm it came first. |
| | AV-09 Finding-to-conclusion gap scan | Extended | Moving from "what it shows" to "what it means". |
| C. Pressure test | AV-07 Argue against my conclusion | Core | Find the weakest point first. |
| | AV-08 Evaluate alternate explanations | Extended | Decide which alternatives belong in the brief. |
| D. Look ahead and triage | AV-12 Project scenarios | Extended | Leadership needs "what if nothing changes". |
| | AV-13 Recommend actions | Extended | Turn diagnosis into ranked options. |
| | AV-14 Confidence triage of findings | Extended — **run at Core depth too if more than 3 findings** | Final gate before deliver-brief. |

**AV-06 Specific challenge** is not in the sequence. Use it whenever the analyst doubts a
result ("are you sure?", "that seems wrong"): replace generic pushback with AV-06 on the
specific claim, and hold the position unless new facts appear.

## Stage-specific rules

- **One finding at a time.** In passes B and C, work finding by finding; draft [FINDING] or
  [CONCLUSION] from the scorecard and confirm with the analyst in one question that lists
  all findings to test (multi-select) rather than one question per finding.
- **Update the scorecard after every prompt.** Change the evidence tag, record count,
  checks run, and status. Narrow the wording of a finding when AV-09 or AV-05 narrows it.
- **Address now vs. acknowledge.** When AV-07 or AV-08 names a gap: if the loaded data can
  close it, run the check now; if not, record it as an open item with an owner (from the
  prepare-data owner list) and keep the finding as a hypothesis.
- **Human check before recommending.** Before AV-13, ask the analyst whether anyone outside
  the data (operations, sales, IT, market) has context on recent changes. Record the answer.
  AV-13 actions must each trace to a finding tagged at the top evidence level or be labeled
  as conditional on an open check.
- **Projections.** Present AV-12 as low / expected / high ranges, never a single number.
- **Triage uses the evidence bar.** AV-14 must use `variables.evidence_bar` from the profile
  when deciding "share now". Write the final status of each finding to the scorecard.
- **Cost watch.** This is the most expensive stage. If redos for a single prompt reach 2,
  stop and ask the analyst whether to change the approach instead of retrying.

## Close the stage

1. Update `run_state.stages_completed` with "3 Analyze and verify".
2. Tell the analyst in 3–5 lines: findings by status (share now / hypothesis / hold),
   the strongest alternate explanation still open, and any check that could change the story.
3. Recommend **Communicate and operationalize** next (deliver-brief). Wait for go-ahead.
