# Playbook Conventions (shared by every Playbook skill)

Every stage skill reads this file before running a prompt. It defines the project folder, the
client/analyst split, how placeholders are filled, how runs are logged, and the standing
review stance.

## 1. Project folder

Each project lives in one folder inside the analyst's working directory:

```
<client-slug>_<project-slug>_<YYYY-MM-DD>/
  01_deliverables/            CLIENT-FACING. Safe to share as-is.
    pre-read-brief.md
    anticipated-questions.md        (when produced)
    working-hypothesis.md           (when produced)
    action-scope-note.md            (when produced)
  _analyst-only/              NEVER SHARE. Costs, logs, raw responses, internal checks.
    README-DO-NOT-SHARE.md
    project-profile.json            interview answers + run state
    project-instructions.md         standing reviewer stance + context brief
    cost-plan.md                    METER estimate before work starts
    cost-audit.md                   METER review after work ends
    prompt-log.csv                  one row per prompt run
    verification-scorecard.md       running list of findings and their status
    responses/                      raw output of every prompt run
      001_INT-03.md
      002_AV-02.md
```

Locate the folder by finding `_analyst-only/project-profile.json` in the working directory.
If several exist, ask which project to use. If none exists, hand off to the start-project skill.

## 2. Client-facing vs analyst-only

Files in `01_deliverables/` must never contain:

- Costs, token counts, model names, METER scores or rates
- Prompt IDs (INT-01, AV-07 …), skill names, or the words "prompt", "Lite", "Full", "tier", "depth"
- Raw AI transcripts, red-flag notes, or reviewer commentary
- Personal, confidential, or regulated data flagged during the governance check
- Any mention that a checklist or harness was used. Write as the analyst's own work.

Deliverables may describe confidence in plain words ("confirmed", "directional, pending X").

Everything else (responses, scorecard, logs, costs) goes to `_analyst-only/`.
Before writing any deliverable, scan it for the forbidden items above and remove them.

## 3. Filling placeholders

Each prompt has bracketed placeholders. Resolve them in this order:

1. **Project profile.** Use the value from `project-profile.json` (`variables` block).
2. **Prior outputs.** For run-level values (FINDING, CONCLUSION, CLAIM, CAUSE …), draft the
   value from the latest verified responses or the scorecard, then show it to the analyst
   and ask them to confirm or edit it in one question.
3. **Ask.** Ask with AskUserQuestion, in the wording from `variables.md`. Offer 2–3 options
   drawn from the data or profile when possible; the analyst can always type their own.
   Ask at most 4 questions per round. Never ask for a value already in the profile.

Save every newly answered project-level value back to the profile so it is never asked twice.
Show the fully filled prompt to the analyst before running it only if they set
`"show_prompts": true` in the profile.

## 4. Running and logging a prompt

For every prompt run:

1. Fill placeholders (section 3).
2. Run the prompt against the data and context in the conversation. Apply the reviewer stance
   (section 6) and the evidence tags in the profile.
3. Judge the output against the prompt's **Good / Red flag** line. If a red flag appears,
   say so to the analyst and offer one targeted redo. Count every redo.
4. Save the response to `_analyst-only/responses/<NNN>_<PROMPT-ID>.md` with a header:
   prompt ID, name, stage, timestamp, filled prompt, response, red-flag check result.
5. Append a row to `_analyst-only/prompt-log.csv` (columns in `templates/prompt-log.csv`).
   Estimate tokens at roughly 4 characters per token.
6. Update `_analyst-only/verification-scorecard.md` when a finding is added, re-labeled,
   confirmed, narrowed, or dropped.
7. Update `run_state` in the profile (`last_stage`, `last_prompt`, `sequence`).

Never write logs, costs, or responses into `01_deliverables/`.

## 5. Regulatory and sensitivity guardrail (every stage)

Before running any prompt that touches data, check the profile's `regulatory` list
(e.g., HIPAA, GDPR, CCPA, GLBA, FERPA, PCI DSS, internal policy). If the data or the
requested output could expose regulated or personal fields:

- Stop and name the specific fields and the rule they fall under.
- Suggest masking, aggregation, or dropping the field, and the minimum extract needed.
- Do not copy record-level personal data into any response file or deliverable.
- Note the guardrail decision in the scorecard and the log ("Notes" column).

This is a prompt to think, not legal advice; say so the first time it triggers in a project.

## 6. Standing reviewer stance

Apply this stance in every stage (it is the content of the project instructions file):

- Act as a critical reviewer, not a cheerleader. First move on any analysis, hypothesis, or
  question: test it. Name an assumption that may not hold, point out what is missing, or ask
  the question that exposes the gap. Only then offer support.
- Tag every finding with an evidence level from the profile. Defaults:
  `[Confirmed]` the data directly shows it · `[Probable]` a reasonable inference from the data ·
  `[Speculative]` filling a gap the data does not cover.
  When a prompt asks for `[Supported]` / `[Inferred]`, map Supported → Confirmed and
  Inferred → Probable or Speculative.
- Skip compliments and filler agreement. Lead with the most important finding.
- Disagree plainly: "I disagree because ___. I would instead ___. The risk is ___."
- If the analyst pushes back without new information, hold the position and explain why.
  Change the answer only on new facts.
- Draw conclusions only from the data provided. Label outside knowledge and name its source.
- Call co-movement a correlation unless a mechanism is named.
- Report how many records each finding rests on.

## 7. Depth

The profile's `depth` is `core` or `extended` (internal names; never shown in deliverables).

- **core**: run prompts marked Core in the stage reference.
- **extended**: run Core plus Extended prompts.

The analyst can add or skip any single prompt at any time. Record skips in the log with
result "Skipped".

## 8. Stage map

| Stage | Skill | Prompt IDs |
|---|---|---|
| 0 Setup | start-project | SET-01, SET-02 |
| 1 Frame | frame-question | INT-01 – INT-04 |
| 2 Prepare and govern | prepare-data | DR-01 – DR-04 |
| 3 Analyze and verify | verify-findings | AV-01 – AV-14 |
| 4 Communicate and operationalize | deliver-brief | CO-01 – CO-05 |
| Cost and log | cost-and-log | METER plan, audit, close-out |

At the end of each stage, summarize in 3–5 lines what changed in the scorecard and suggest
the next stage. Do not start the next stage without the analyst's go-ahead.
