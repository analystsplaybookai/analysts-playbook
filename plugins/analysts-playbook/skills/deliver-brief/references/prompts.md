# Prompt Library — Stage 4 Communicate and operationalize

Prompts for this stage, in run order. **Core** prompts run at every depth; **Extended** prompts run only when the profile's depth is `extended` (or the analyst asks for one).

Fill placeholders using conventions §3 and the variable registry. Judge every output against its Good / Red flag line before saving.


---

## CO-01 · Draft a one-page pre-read brief  
**Depth:** Core · **Pairs with:** CO-02, CO-03

**When to use:** Turn verified work into a stand-alone document stakeholders read before the meeting.

**Placeholders:** [AUDIENCE]

**Prompt:**

```text
Draft a one-page pre-read using only what we've verified in this conversation. Four sections:
1. Finding (1–2 sentences): the metric, the segment, the time period, the size of change. No causes or advice.
2. Confidence (1–2 sentences): what is confirmed, what is directional, and what evidence would close the gap.
3. Recommendation (1–3 sentences): a specific action, an owner, and a date. Nothing beyond what the evidence supports.
4. Rationale (short paragraph): the alternatives we considered and why the evidence points away from them or leaves them open.
Audience: [AUDIENCE]. It must make sense without me in the room.
```

**Good / Red flag:** Good: Finding contains no 'because'. Red flag: recommendation like 'investigate retention' with no action, owner, or date.


---

## CO-02 · Brief readiness audit  
**Depth:** Extended · **Pairs with:** CO-01

**When to use:** Run on your draft brief before sending.

**Placeholders:** [PASTE BRIEF]

**Prompt:**

```text
Audit this brief: [PASTE BRIEF]. Check and report Pass/Fix for each: (1) Finding names a metric, segment, and time period and contains no cause; (2) Confidence statement separates confirmed from directional; (3) Recommendation is specific, dated, owned, and traceable to the finding; (4) the scope and cost of the action match the scope of the evidence; (5) Rationale addresses at least one alternative explanation; (6) a reader with no context could act on it in five minutes. Suggest exact rewrites for anything marked Fix.
```

**Good / Red flag:** Good: line-level rewrites. Red flag: passes a brief whose recommendation outruns its evidence.


---

## CO-03 · Anticipated questions & prepared answers  
**Depth:** Extended · **Pairs with:** AV-07, AV-08

**When to use:** Meeting prep – so you can say 'I checked that' instead of being caught flat.

**Placeholders:** [PASTE OR SUMMARIZE], number, [ROLES]

**Prompt:**

```text
Based on this brief and our analysis: [PASTE OR SUMMARIZE]. List the [5] toughest questions or objections stakeholders in [ROLES] are likely to raise. For each, write a 2-sentence answer grounded in what we verified, or – if we can't answer yet – an honest statement of what is unknown and how we'd find out.
```

**Good / Red flag:** Good: answers cite specific checks already run. Red flag: confident answers to questions the data can't address.


---

## CO-04 · Match action scope to evidence scope  
**Depth:** Extended · **Pairs with:** AV-09, CO-01

**When to use:** Use before proposing a broad or expensive initiative.

**Placeholders:** [ACTION], scope, [COST], [FINDING]

**Prompt:**

```text
Proposed action: [ACTION] (scope: [WHO/WHAT IT AFFECTS], est. cost/effort: [COST]). Supporting evidence: [FINDING]. Is the evidence as broad as the action? If the problem is concentrated in one segment, region, product, or channel, propose a smaller targeted version of the action and the trigger that would justify scaling up.
```

**Good / Red flag:** Good: a targeted pilot plus a scale-up trigger. Red flag: company-wide fix for a single-segment finding.


---

## CO-05 · Write up a working hypothesis  
**Depth:** Extended · **Pairs with:** AV-14, CO-01

**When to use:** Use when your best explanation isn't proven yet but belongs in the brief.

**Placeholders:** [EXPLANATION]

**Prompt:**

```text
Write our leading explanation as a clearly labeled working hypothesis: [EXPLANATION]. Include: why it is the strongest candidate, what it would take to confirm it (data, owner, time needed), what alternatives remain open, and the low-risk next step we can take now while confirming.
```

**Good / Red flag:** Good: confirmable next step with an owner. Red flag: hypothesis worded as fact.
