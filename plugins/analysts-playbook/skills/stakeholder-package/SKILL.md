---
name: stakeholder-package
description: >
  This skill should be used when an analyst wants to "build the stakeholder package",
  "make a deck from the brief", "draft the email to the stakeholder", "make a video of the
  findings", "turn the deliverables into slides", or "build the prompts and replies review".
  Turns a project's approved deliverables into a slide deck, an email draft, and a
  voiceover video, with reconciliation and review steps. Working files stay analyst-only.
  Optional stage 5 of the Analyst's Playbook, run after deliver-brief.
metadata:
  version: "0.2.0"
---

# Stage 5 (optional): Stakeholder Package

Builds a stakeholder package from a Playbook project folder: a short slide deck, an email
draft to the decision owner, and a narrated slideshow video (voiceover only), plus a review
document of the analysis prompts and replies.

## Before running

1. Read `../start-project/references/conventions.md` and apply it throughout. Section 2
   (client-facing rules and the whole-folder screen) is mandatory.
2. Load the project profile. If deliver-brief has not finished (no `pre-read-brief.md` in
   `01_deliverables/`), say so and recommend running deliver-brief first.

## Folder rules (apply to every step)

| Folder | What goes there |
|---|---|
| `01_deliverables/` | Only files the analyst has **approved for the client**. Read from it; write to it only in step 8. |
| `_analyst-only/` | Everything this skill produces until the analyst approves it, and everything that never goes to the client. |

- Write all working outputs to `_analyst-only/stakeholder-package/`: the email draft, the
  video prompt (with version history and project id), and the QA notes.
- Write the review document to `_analyst-only/analysis-prompts-and-replies.html`.
- **Never** write prompts, prompt logs, review or QA documents, costs, extracts, or video
  prompts to `01_deliverables/`, even after approval.
- Before saving any file, check its destination against this table. If a file is in the
  wrong folder, say so and ask the analyst to delete it; don't leave a duplicate unmentioned.
- The deck is an artifact link, private until the analyst shares it. Don't put the link in
  anything client-facing, and don't export the deck into `01_deliverables/` until step 8.

## Inputs

1. `01_deliverables/`: the approved analysis outputs (pre-read brief, working hypothesis,
   action scope note, anticipated questions).
2. `_analyst-only/project-profile.json`: organization, decision, audience, requester,
   as-of date, evidence bar.
3. `_analyst-only/responses/*.md` and `_analyst-only/prompt-log.csv`: the analysis prompts
   and replies.

If any of these are missing, say which and continue with what's there.

## Steps

### 1. Read and plan
Read every file in `01_deliverables/` and the project profile. Note: who asked (name, role),
the original question, the decision and its deadline, the recommendation with owners and
dates. Flag any deadline that is already past as of today.

### 2. Build the deck
Use the session's slide-deck artifact type if one is available; otherwise a .pptx. Six
slides, answer first:
1. Cover: the answer in one sentence.
2. The original ask: who asked (name and role, from the profile), what they asked
   (paraphrase without quotation marks unless the exact words are on file), the decision
   riding on it, and how the question was framed neutrally.
3. The problem: the headline numbers, against every baseline in the profile.
4. The hypothesis: the evidence, labeled as a hypothesis if not confirmed.
5. The solution: confirmed vs. still open.
6. The recommendation: action, owner, date; triggers to scale (only trigger findings that
   cleared the evidence bar; others are "watch" items).

Speaker notes on every slide; they become the video narration. Use only figures from the
inputs. Then review the deck in the live viewer, not only a local render: content fills the
slide (no empty bottom half), text is readable, tables keep their styling. Fix and re-check.

### 3. Draft the email
To the decision owner from the profile, copying the action owners. Bottom line first, then
problem, hypothesis, evidence, recommendation with owners and dates, and the scale-up
triggers. Refer to the deck as attached or shared, without an artifact link. Save to
`_analyst-only/stakeholder-package/stakeholder-email-draft.md`. Never send it.

### 4. Reconcile email and slides
- Extract every figure from the slide text and the email; every figure in the email must
  appear on the slides.
- Check that owners, dates, recommendations and triggers match line for line.
- Add to the email anything the deck treats as a key point (cost, comparison groups,
  decision owners).
- Check that the slide count and video length stated in the email are correct (recheck
  after steps 5–6).

Record the result in the QA notes.

### 5. Generate the video
If a video connector is available (for example HeyGen HyperFrames), make one scene per
slide, in deck order, with the exact on-screen text and the speaker notes as narration.
Voiceover only: no avatar or presenter. Include this requirement in the prompt: every
headline, label, card, bullet and icon must be fully visible (at least 4.5:1 contrast) for
the whole scene, with no dark text or icons on dark backgrounds. Save the prompt and the
project id to `_analyst-only/stakeholder-package/video-prompt.txt`. If no video connector
is available, save the prompt there for the analyst to use and skip to step 7.

Don't delete video projects until the package is approved: deleting the editing session
means any later change requires a full rebuild.

### 6. Review the video (required)
Open the video and step through every scene **during playback**, not only paused frames.
Check:
- every headline, label, card and bullet is visible;
- figures, owners and dates match the deck;
- scene order matches the deck, with no presenter;
- names are pronounced correctly;
- nothing is invented;
- the actual length.

Fix anything wrong with a follow-up edit to the same project, then review again. Update the
email's stated length to the actual length. If the video can't be opened, give the analyst
this checklist instead and mark the review pending.

### 7. Build the review document
`_analyst-only/analysis-prompts-and-replies.html`: an ANALYST-ONLY banner; totals (prompts,
red flags, redos, Revise items, estimated cost from the prompt log); an index by stage; and
for each prompt, the prompt and reply side by side with its log notes, collapsible. Build it
from `_analyst-only/responses/` and the prompt log, not from the chat. Show the prompts and
replies as recorded, unedited. End with the QA section: deck review, email/slide
reconciliation, video review, past-due dates, and anything misfiled.

### 8. Approval and hand-off
Ask the analyst to approve the deck, email and video. Only after approval, copy the
client-facing finals to `01_deliverables/`:
- the email (final text),
- the video MP4,
- a deck export (PDF or PPTX) if the analyst wants one.

Nothing else goes there. Run the whole-folder screen (conventions §2) on `01_deliverables/`
after copying. The analyst sends the email.

## Finish

Update `run_state.stages_completed` with "5 Stakeholder package". Report in a few lines:
what was built, what the reviews found and fixed, where each file is, and anything the
analyst must do (share the deck, download the MP4, delete misfiled files).
