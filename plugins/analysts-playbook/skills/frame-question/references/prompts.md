# Prompt Library — Stage 1 Frame

Prompts for this stage, in run order. **Core** prompts run at every depth; **Extended** prompts run only when the profile's depth is `extended` (or the analyst asks for one).

Fill placeholders using conventions §3 and the variable registry. Judge every output against its Good / Red flag line before saving.


---

## INT-01 · Premise check before 'why'  
**Depth:** Core · **Pairs with:** INT-02, AV-01

**When to use:** Use whenever a request starts with 'why did X happen' – confirm X actually happened before explaining it.

**Placeholders:** [METRIC], direction, [AMOUNT], [PERIOD]

**Prompt:**

```text
Before explaining any cause, confirm the premise. I believe [METRIC] [rose/fell] by [AMOUNT] during [PERIOD]. Using the data provided: (1) does the data actually show this change, and against what baseline; (2) how many records is it based on; (3) is the change concentrated in any segment? If the premise does not hold, stop and tell me instead of explaining it.
```

**Good / Red flag:** Good: AI may push back that the change is smaller, absent, or isolated. Red flag: AI jumps straight to causes.


---

## INT-02 · De-bias a leading question  
**Depth:** Extended · **Pairs with:** INT-01, AV-07

**When to use:** Use when you already suspect the answer and want to avoid steering AI toward it.

**Placeholders:** [YOUR QUESTION]

**Prompt:**

```text
Here is the question I was about to ask: "[YOUR QUESTION]". Before answering, (1) point out any assumption or expected answer built into how I phrased it, (2) rewrite it as a neutral question that could be answered either way, and (3) answer the neutral version, weighing evidence for and against my original hunch.
```

**Good / Red flag:** Good: the rewrite removes the presumed cause. Red flag: neutral version still names your suspect as the frame.


---

## INT-03 · Scoped analysis request  
**Depth:** Core · **Pairs with:** AV-02, DR-01

**When to use:** Default structure for any first-pass analysis instead of 'tell me what's going on'.

**Placeholders:** [METRIC], [POPULATION], [TIME WINDOW], [BASELINE]

**Prompt:**

```text
Analyze [METRIC] for [POPULATION / SEGMENT DEFINITION] over [TIME WINDOW], compared with [BASELINE]. Flag any meaningful trends or breaks in the pattern. List every assumption you made about the data (definitions, exclusions, how you handled missing values) and the number of records behind each finding.
```

**Good / Red flag:** Good: assumptions and record counts listed. Red flag: AI silently picks its own window or segment.


---

## INT-04 · Set the decision & confidence bar  
**Depth:** Extended · **Pairs with:** AV-14, CO-04

**When to use:** Use at intake (and revisit before sharing) to decide how sure you need to be for the action at stake.

**Placeholders:** [DECISION]

**Prompt:**

```text
This analysis will inform the following decision: [DECISION]. Help me set the evidence bar: (1) what does it cost if we act and are wrong, and if we wait and are wrong; (2) is this a reversible or hard-to-reverse action; (3) given that, what level of certainty is sufficient – a directional signal, a probable finding, or a confirmed finding; (4) what is the smallest piece of evidence that would clear that bar?
```

**Good / Red flag:** Good: a clear 'good-enough' threshold tied to the action. Red flag: AI demands certainty for a cheap, reversible step.
