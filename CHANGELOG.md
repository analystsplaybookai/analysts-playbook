# Changelog

## 0.3.0 — 2026-11-24

The four dials: Cost, Trust, Fluency, Risk.

- Every prompt carries a **Dial** tag (the dial it moves most, plus any clear overlap);
  conventions §9 defines the dials.
- **Two-way risk scoring at setup:** six intake factors (impact, reversibility, data
  sensitivity, audience, wrong data in, decision trigger). The analyst scores first, then the
  AI; a mismatch triggers targeted follow-ups, and the higher tier is the default. Sample
  projects for each tier in `risk-tiers.md`.
- **The tier sets the controls:** low = self-check; medium = independent recompute of
  headline figures plus a specific challenge; high = adds a named reviewer's approval before
  anything reaches `01_deliverables/`. Every stage rescores if scope, source, or audience changes.
- **ROI in the cost audit** when a hold-out baseline exists; otherwise the audit says ROI
  wasn't measured instead of guessing.
- Fluency cap on risk tiers is recorded as "not set" until fluency tiers are defined.

## 0.2.0 — 2026-11-24

Fixes from the first full test run (a five-location no-show analysis), plus a new stage.

**Analyze and verify**
- Diagnosis now breaks the change down across every dimension in the data, removes the
  leading driver, and checks what's left, so a second driver can't be missed.
- A cause suggested by comments or interviews must be checked for its pattern in the
  structured data before it is called unconfirmed.
- "Same customer" comparisons must be paired per unit, then averaged; pooled rows don't count.
- Impact sizing covers the whole change as a range, split by driver with an "unexplained" share.
- Numbers that become decision triggers need the top evidence level; otherwise they are
  "watch" items only.

**Prepare and govern**
- New analyst-only `data-quality-log.md`; every logged item must reach the brief or be
  marked not material.
- All counts come from one cleaned extract.
- Like-for-like check adds paired-per-unit math and an exposure-timing check.

**Communicate and operationalize**
- The brief reports every baseline in the profile, and a claim-to-number check tests every
  comparative word against the figures.
- The brief audit has two new checks (eight in total).
- The client-facing screen now runs on the whole `01_deliverables/` folder whenever anything
  is written there, blocks AI-tool links and tool names, and sends working files
  (review logs, tool prompts, unapproved drafts) to `_analyst-only/` by default.

**New: stakeholder-package skill (optional stage 5)**
- Deck, email draft, and voiceover video from the approved brief; email/slide
  reconciliation; required video review; prompts-and-replies review document. All working
  files stay analyst-only until the analyst approves the finals.

## 0.1.0 — 2026-11-16

Initial release.
