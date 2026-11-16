---
name: deliver-brief
description: >
  This skill should be used when an analyst wants to "write the brief", "draft the
  pre-read", "prepare for the meeting", "what questions will they ask", "write up the
  hypothesis", "is my recommendation too big", "audit my brief", or turn verified findings
  into a stakeholder-ready deliverable. Writes only client-safe files to 01_deliverables.
  Stage 4 (Communicate and operationalize) of the Analyst's Playbook.
metadata:
  version: "0.1.0"
---

# Stage 4: Communicate and Operationalize

Turn verified work into a stand-alone brief stakeholders can act on, sized to the evidence,
with prepared answers to the hard questions.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout. Section 2
   (client-facing rules) is mandatory for every file this skill writes.
2. Read `../start-project/references/variables.md` for placeholder sources.
3. Load the project profile and the scorecard. If no finding has status "Share now" or
   "Share as hypothesis", tell the analyst and recommend finishing verify-findings first.
4. Read `references/prompts.md` for the exact prompt text, placeholders, and checks.
5. Read `references/deliverable-format.md` for file layout and header rules.

## Run order

| Order | Prompt | Depth | Output file (client-facing) |
|---|---|---|---|
| 1 | CO-04 Match action scope to evidence scope | Extended — **run at Core depth if the recommendation is costly or organization-wide** | `01_deliverables/action-scope-note.md` (only if the analyst wants it shared) |
| 2 | CO-05 Write up a working hypothesis | Extended | `01_deliverables/working-hypothesis.md` (when a "Share as hypothesis" finding exists) |
| 3 | CO-01 Draft a one-page pre-read brief | Core | `01_deliverables/pre-read-brief.md` |
| 4 | CO-02 Brief readiness audit | Extended — **always run a silent check of its six criteria at Core depth** | Fixes applied to the brief; audit saved analyst-only |
| 5 | CO-03 Anticipated questions and prepared answers | Extended | `01_deliverables/anticipated-questions.md` |

## Stage-specific rules

- **Only verified content.** Build deliverables from scorecard findings with status
  "Share now" (stated as findings) or "Share as hypothesis" (stated as hypotheses with the
  evidence that would confirm them). Never include "Hold" or "Dropped" findings.
- **Two copies of every response.** Save the raw prompt response to
  `_analyst-only/responses/` as usual. Write a cleaned client version to `01_deliverables/`
  following `references/deliverable-format.md`.
- **Screen before writing.** Before saving any file to `01_deliverables/`, check it against
  conventions §2 (no costs, prompt IDs, tool or tier words, raw transcripts, regulated data).
  Fix anything found, then save.
- **Brief audit.** Run CO-02's six checks on the draft brief. Apply rewrites marked Fix. Save
  the audit result to `_analyst-only/responses/` only.
- **Scope match.** If CO-04 recommends a smaller pilot, update the brief's recommendation to
  the pilot plus its scale-up trigger, and tell the analyst.
- **Word output.** If the profile's `deliverable_format` is `docx`, also produce a .docx of
  each deliverable (use a docx skill if available) alongside the markdown.

## Close the project

1. Update `run_state.stages_completed` with "4 Communicate and operationalize".
2. Invoke the cost-and-log skill in **audit** mode to write `_analyst-only/cost-audit.md`.
3. Tell the analyst: the deliverables written (file names only), anything held back and why,
   and the one-line cost audit summary. Remind them that only `01_deliverables/` is for sharing.
