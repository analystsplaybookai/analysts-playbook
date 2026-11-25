# Prompt Library — Stage 2 Prepare and govern

Prompts for this stage, in run order. **Core** prompts run at every depth; **Extended** prompts run only when the profile's depth is `extended` (or the analyst asks for one).

Fill placeholders using conventions §3 and the variable registry. Judge every output against its Good / Red flag line before saving.


---

## DR-01 · Data coverage check  
**Depth:** Core · **Pairs with:** AV-01, SET-02

**When to use:** Use after any summary of text, reviews, surveys, or large files – confirms AI used the whole dataset, not a slice.

**Placeholders:** None

**Prompt:**

```text
Before I rely on this, confirm your coverage: (1) how many records/rows/responses exist in the data I provided, and how many did you actually use; (2) did you filter, sample, truncate, or exclude anything – and why; (3) did a handful of extreme or unusually long entries drive the result? If you did not use all of it, rerun on the complete set and tell me what changed.
```

**Good / Red flag:** Good: used = total, or exclusions are explained. Red flag: summary quotes only vivid outliers.


---

## DR-02 · Missing-data & owner inventory  
**Depth:** Extended · **Pairs with:** AV-08, CO-05

**When to use:** Use when a finding needs a cause the dataset can't show; turns the gap into a to-do list with human owners.

**Placeholders:** [FINDING], [INDUSTRY]

**Prompt:**

```text
For the finding "[FINDING]": (1) what data outside this dataset would be needed to fully explain it; (2) rank those gaps by how much they could change the conclusion; (3) for the top three, which team or system in a typical [INDUSTRY] organization would own that data; (4) what one question should I ask each owner? Note anything that changed in operations, systems, pricing, or the market that would not appear in this data.
```

**Good / Red flag:** Good: names systems (CRM, email, support, finance, competitors) and people to ask. Red flag: only suggests 'more of the same data'.


---

## DR-03 · Like-for-like comparison check  
**Depth:** Extended · **Pairs with:** AV-01

**When to use:** Use before comparing two groups, channels, regions, or periods – makes sure the comparison is fair.

**Placeholders:** [GROUP A], [GROUP B], [METRIC]

**Prompt:**

```text
I'm comparing [GROUP A] with [GROUP B] on [METRIC]. Before accepting the difference, test whether they are comparable: do they differ in mix (customer type, issue complexity, size, tenure, region), volume, measurement method, or time period? List each difference you can see in the data, estimate whether it could explain the gap, and suggest a fairer comparison (e.g., within matched segments).
If the same units (customers, patients, stores) appear in both groups, calculate the difference for each unit first and then average it; do not pool all rows. Also check whether the timing of being in each group lines up with something else, such as how far along the unit is, the calendar month, or a price or policy change.
```

**Good / Red flag:** Good: identifies mix differences, proposes a matched comparison, and computes paired differences per unit. Red flag: treats raw averages as apples-to-apples, or calls a pooled comparison "same customers".


---

## DR-04 · Sensitivity, cost & repeatability check  
**Depth:** Extended · **Pairs with:** SET-02

**When to use:** Governance gate before uploading data or reusing a result in a recurring report.

**Placeholders:** [DATASET DESCRIPTION], [PURPOSE], [FIELD LIST]

**Prompt:**

```text
I plan to use AI on [DATASET DESCRIPTION] for [PURPOSE]. Review the fields I've listed: [FIELD LIST]. (1) Flag any personal, confidential, or regulated fields and suggest how to mask, aggregate, or drop them without losing the analysis; (2) suggest the smallest extract that still answers the question; (3) because AI responses can vary run to run, tell me which parts of this analysis should be done with a fixed calculation (formula, SQL, script) so results are repeatable, and which parts are fine as AI interpretation.
```

**Good / Red flag:** Good: clear mask/drop list and a split between deterministic calcs vs AI narrative. Red flag: suggests uploading raw identifiers.
