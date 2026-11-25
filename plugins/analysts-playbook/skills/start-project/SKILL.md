---
name: start-project
description: >
  This skill should be used when an analyst wants to "start a new analysis project",
  "set up the playbook", "start the playbook", "new client analysis", "interview me for a
  project", "resume my analysis project", or begins any data analysis engagement with the
  Analyst's Playbook. It interviews the analyst for every project variable, builds the
  project folder with a client-facing deliverables area and an analyst-only area for costs,
  logs and raw responses, and writes the cost plan before analysis starts.
metadata:
  version: "0.3.0"
---

# Start a Playbook Project

Set up a new analysis project through a short interview, or resume an existing one.
The interview collects every project-level prompt variable once, so no later stage asks again.

Read `references/conventions.md` (folder layout, client/analyst split, logging, guardrails)
and `references/variables.md` (every variable and the exact interview questions) before
starting.

## Step 1: Check for an existing project

Search the working directory for `_analyst-only/project-profile.json`.

- **Found one:** read it, summarize the project in 3 lines (organization, question, last stage
  completed), and ask: resume this project, or start a new one?
  On resume, skip to Step 6 and recommend the next stage from `run_state`.
- **Found none:** continue to Step 2.

If no working folder is connected, ask the analyst to connect or choose a folder before
writing anything.

## Step 2: Open the interview

Tell the analyst in one or two sentences: about 5 short rounds of questions, answers are
saved so they are asked only once, and they can skip any question and fill it later.

If the analyst's opening message already contains answers (organization, metric, question,
data files), pre-fill those and do not ask for them again.

If data files are present in the working folder, list them (names, row counts if quick to
read) so the data inventory question can offer them as options.

## Step 3: Run the interview rounds

Run Rounds 1–4 from `references/variables.md` with AskUserQuestion, one round per call,
up to 4 questions per round. Use the question wording and options from the registry.
Tailor options to what is already known (e.g., offer metric options from data column names).

After Round 4, show the Round 5 defaults in a compact list and ask one question:
"Keep these settings or change any?" Only ask the individual Round 5 questions if the analyst
wants changes.

Rules:
- Free-text answers (business context, data inventory) may be asked in plain chat instead
  of AskUserQuestion when options do not make sense.
- If the analyst skips a question, leave the value empty; stage skills will ask when needed.
- Ask about `regulatory` even if the analyst seems rushed. It drives the guardrail in every stage.
- Do not mention "Lite", "Full", or "tier". Use "Core" and "Extended" when asking about depth.

## Step 4: Confirm

Show a short summary table of the answers (variable → value) and ask the analyst to confirm
or correct. Apply corrections.

## Step 4b: Score the risk tier

Read `references/risk-tiers.md` and run two-way scoring: intake questions, the analyst's
tier first, then the AI's tier with a one-line reason, follow-ups on any mismatch, higher
tier by default. Tell the analyst in one line what the final tier means for later stages
(conventions §10).

## Step 5: Build the project folder

Create the folder `<client-slug>_<project-slug>_<YYYY-MM-DD>/` with the layout in
`references/conventions.md`. Slugs are lowercase-hyphenated; derive the project slug from the
metric and decision (e.g., `first-visit-fix-rate-q3`). Then write:

1. `_analyst-only/project-profile.json` from `templates/project-profile.json`, filled.
2. `_analyst-only/project-instructions.md` from `templates/project-instructions.md`, with every
   `{{…}}` replaced. This is SET-01 (reviewer stance) plus SET-02 (context brief) combined.
3. `_analyst-only/README-DO-NOT-SHARE.md`, `prompt-log.csv`, `verification-scorecard.md`,
   `data-quality-log.md` from templates, unchanged.
4. Empty folders `_analyst-only/responses/` and `01_deliverables/`.
5. Log the setup as rows in `prompt-log.csv`: SET-01 "Standing skeptic instructions" and
   SET-02 "Project context brief", stage "0 Setup", result "Written to project-instructions.md";
   and RISK-01 "Risk tier", result "<final tier>".

Apply the reviewer stance from `project-instructions.md` for the rest of the conversation.

## Step 6: Plan cost, then hand off

1. Invoke the cost-and-log skill in **plan** mode to write `_analyst-only/cost-plan.md`
   (METER estimate for this depth and data size). Show the analyst only the one-line summary
   and any recommendation that would cut cost (e.g., a smaller extract).
2. Recommend the next stage, normally **Frame** (frame-question skill). Mention that if the
   question is already sharply framed and the data is loaded, they can go straight to
   **Prepare and govern**.
3. Wait for the analyst's go-ahead before starting the next stage.

## Final message to the analyst

Keep it to: the risk tier and its controls (one line), where the project folder is, what is client-facing vs analyst-only (one line
each), the cost-plan one-liner, and the recommended next step.
