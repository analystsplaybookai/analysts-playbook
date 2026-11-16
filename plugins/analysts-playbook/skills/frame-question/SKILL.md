---
name: frame-question
description: >
  This skill should be used when an analyst needs to "frame the question", "scope the
  analysis", "check the premise", "why did this metric drop", "why did X go up", "set the
  confidence bar", or wants to turn a stakeholder request into a well-scoped analysis
  question before touching the data. Stage 1 (Frame) of the Analyst's Playbook.
metadata:
  version: "0.1.0"
---

# Stage 1: Frame the Question

Turn the request into a question the data can answer, confirm the premise is real, and set
how sure the analysis needs to be before anyone acts.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout.
2. Read `../start-project/references/variables.md` for placeholder sources.
3. Load the project: find `_analyst-only/project-profile.json`. If none exists, invoke the
   start-project skill first and stop.
4. Read `references/prompts.md` for the exact prompt text, placeholders, and checks.

## Run order

| Order | Prompt | Depth | Run when |
|---|---|---|---|
| 1 | INT-02 De-bias a leading question | Extended | The analyst's question presumes a cause or answer. At Core depth, still offer it in one line if the question is clearly leading. |
| 2 | INT-01 Premise check before "why" | Core | The question asks why something happened. Skip if the question is not causal and log as Skipped. |
| 3 | INT-03 Scoped analysis request | Core | Always. This is the default first pass. |
| 4 | INT-04 Set the decision and confidence bar | Extended | A decision is named in the profile. |

## Stage-specific rules

- **Premise fails → stop.** If INT-01 shows the change is absent, smaller than claimed, or
  isolated to one segment, stop the stage. Report what the data actually shows and ask the
  analyst whether to re-scope the question (update `metric`, `population`, or `time_window`
  in the profile) before continuing.
- **Rewritten question.** If INT-02 produces a neutral rewrite, ask the analyst whether to
  adopt it. If yes, use it for every later prompt and record it in the profile as
  `variables.framed_question`.
- **First findings.** Add each finding from INT-03 to the scorecard with its evidence tag and
  record count, status Open.
- **Confidence bar.** Save the INT-04 result (required certainty and the smallest sufficient
  evidence) to the profile as `variables.evidence_bar`. Verify-findings and deliver-brief use it.
- **Guardrail.** Run the regulatory check (conventions §5) before INT-01 or INT-03 touches data.

## Close the stage

1. Update `run_state.stages_completed` with "1 Frame".
2. Tell the analyst in 3–5 lines: the framed question, whether the premise held, the
   findings now on the scorecard, and the evidence bar.
3. Recommend **Prepare and govern** next (prepare-data). Wait for go-ahead.
