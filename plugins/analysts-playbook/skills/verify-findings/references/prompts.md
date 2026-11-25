# Prompt Library — Stage 3 Analyze and verify

Prompts for this stage, in run order. **Core** prompts run at every depth; **Extended** prompts run only when the profile's depth is `extended` (or the analyst asks for one).

Fill placeholders using conventions §3 and the variable registry. Judge every output against its Good / Red flag line before saving.


---

## AV-01 · Six-point reliability sweep  
**Depth:** Core · **Pairs with:** AV-02, AV-03

**When to use:** Run on any AI finding before trusting it; returns a quick scorecard.

**Placeholders:** [FINDING], [SEGMENT]

**Prompt:**

```text
Audit this finding: "[FINDING]". For each check, give a one-line answer and a Pass / Concern rating:
1. Volume – how many records support it, and is that enough to trust?
2. Cause vs. coincidence – is there a mechanism, or just matching timing?
3. Framing – did my question point toward this answer?
4. Extremes – does it still hold if the top and bottom 5% are removed?
5. Averages – does it still hold when broken out by [SEGMENT]?
6. Circularity – does it tell me something new, or restate the premise?
End with the single check most likely to change the conclusion.
```

**Good / Red flag:** Good: specific numbers per check. Red flag: all six 'Pass' with no evidence.


---

## AV-02 · Surface hidden assumptions  
**Depth:** Core · **Pairs with:** AV-05

**When to use:** Verification step 1 – ask right after any analysis or recommendation.

**Placeholders:** None

**Prompt:**

```text
List every assumption you made in this analysis that I did not explicitly state – about definitions, time frames, which records count, cause and effect, and what 'normal' looks like. For each, say how the conclusion changes if the assumption is wrong.
```

**Good / Red flag:** Good: at least one assumption you hadn't considered. Red flag: 'No assumptions were made.'


---

## AV-03 · Evidence label per finding  
**Depth:** Core · **Pairs with:** CO-01

**When to use:** Verification step 3 – separate what the data shows from what AI inferred.

**Placeholders:** None

**Prompt:**

```text
Go through each finding in this analysis and label it: [Supported] – shown directly in the data (cite the figure), or [Inferred] – a reasonable interpretation the data doesn't prove (say what would confirm it). Present as a table: Finding | Label | Evidence or what would confirm it.
```

**Good / Red flag:** Good: causes land in [Inferred]. Red flag: every line marked [Supported].


---

## AV-04 · Wider-window durability test  
**Depth:** Extended · **Pairs with:** AV-03, AV-14

**When to use:** Use when a pattern comes from a short window (e.g., one quarter) – checks whether it's real or noise.

**Placeholders:** [FINDING], [SHORT WINDOW], [LONGER WINDOW]

**Prompt:**

```text
The finding "[FINDING]" comes from [SHORT WINDOW]. Rerun the identical analysis over [LONGER WINDOW] (and, if available, the same window last year). Report whether the pattern holds, weakens, reverses, or was already present before. Then tell me whether this finding should be called confirmed or still directional.
```

**Good / Red flag:** Good: side-by-side comparison and an explicit confirmed/directional call. Red flag: different method used for the longer window.


---

## AV-05 · Targeted assumption test  
**Depth:** Extended · **Pairs with:** AV-02, AV-08

**When to use:** Use when a first analysis blames a specific cause – tests whether the affected group actually matches it.

**Placeholders:** [FINDING], [SUSPECTED CAUSE], [AFFECTED GROUP]

**Prompt:**

```text
Our first pass links [FINDING] to [SUSPECTED CAUSE]. Before acting, test that link: among the [AFFECTED GROUP], are they actually disproportionately exposed to [SUSPECTED CAUSE] compared with everyone else? Break it out by segment and show the numbers. If the effect is concentrated somewhere else, tell me where.
```

**Good / Red flag:** Good: segment table that either confirms or redirects. Red flag: restates the link without segmenting.


---

## AV-06 · Specific challenge (instead of pushback)  
**Depth:** Core · **Pairs with:** SET-01

**When to use:** Use instead of 'are you sure?' – generic pushback just makes AI change its answer.

**Placeholders:** [CLAIM]

**Prompt:**

```text
Don't change your answer – defend or qualify it. For "[CLAIM]": (1) what specific data supports it; (2) what would have to be true for it to be wrong; (3) which single assumption, if false, breaks it; (4) what quick check would tell us?
```

**Good / Red flag:** Good: names a concrete test. Red flag: AI flips its answer with no new evidence.


---

## AV-07 · Argue against my conclusion  
**Depth:** Core · **Pairs with:** AV-08, CO-03

**When to use:** Stress test – find the weakest point before a stakeholder does.

**Placeholders:** [CONCLUSION]

**Prompt:**

```text
My conclusion: [CONCLUSION]. Using only the data and context already provided, make the strongest case against it. What would a skeptical colleague argue, which data would they point to, and which assumptions would they attack? Finish with the one testable gap that matters most and how to close it.
```

**Good / Red flag:** Good: a specific, testable gap. Red flag: vague 'more data could help'.


---

## AV-08 · Evaluate alternate explanations  
**Depth:** Extended · **Pairs with:** AV-07, CO-01, CO-03

**When to use:** Pressure test – take competing explanations seriously and decide which belong in the brief.

**Placeholders:** [CONCLUSION], data coverage, number of alternatives

**Prompt:**

```text
Primary conclusion: [CONCLUSION – metric, segment, period].
Data behind it: [WHAT THE DATA COVERS AND DOESN'T].
Generate the [2–3] most plausible alternate explanations a stakeholder might raise. For each: (a) what would support it; (b) does my data support it, contradict it, or leave it open; (c) what pattern in the data would we expect to see if it were true versus if my conclusion were true; (d) what extra data would confirm or rule it out; (e) one-sentence verdict: Ruled out / Testable now / Open – monitor.
```

**Good / Red flag:** Good: (c) gives a distinguishing test (e.g., broad vs. concentrated pattern). Red flag: alternatives dismissed without evidence.


---

## AV-09 · Finding-to-conclusion gap scan  
**Depth:** Extended · **Pairs with:** AV-10, CO-04

**When to use:** Use when moving from 'what the data shows' to 'what it means' – catches overreach.

**Placeholders:** [FINDING], [CONCLUSION]

**Prompt:**

```text
Finding (data only): [FINDING]. Conclusion I want to draw: [CONCLUSION]. Check the leap between them for three problems: (1) Scope – does the conclusion apply to more people than the data covers? (2) Timing – could an earlier event be showing up now, rather than the most recent change? (3) Overlap – are two separate issues being treated as one because they happened together? For each, say Yes/No/Unclear with evidence, then rewrite the conclusion so it goes no further than the data supports.
```

**Good / Red flag:** Good: a narrower, rewritten conclusion. Red flag: 'No issues found' on a broad, company-wide claim.


---

## AV-10 · Sequence & lag check  
**Depth:** Extended · **Pairs with:** AV-09

**When to use:** Use when a cause is suspected – confirms the cause came first and allows for delayed effects.

**Placeholders:** [CAUSE], [EFFECT], dates, [LOOKBACK PERIOD]

**Prompt:**

```text
Proposed cause: [CAUSE] (began [DATE]). Effect: [EFFECT] (first visible [DATE]). Using the data, (1) confirm the cause preceded the effect for the affected records; (2) estimate a plausible lag between them; (3) list other changes in the prior [LOOKBACK PERIOD] that could also explain the effect; (4) tell me what record-level timestamp comparison would prove or disprove the sequence.
```

**Good / Red flag:** Good: a specific before/after test. Red flag: assumes the newest event is the cause.


---

## AV-11 · Climb the ladder: diagnose  
**Depth:** Core · **Pairs with:** AV-12

**When to use:** Moves a bare fact ('X is down') to a cause. Level 2 of 4.

**Placeholders:** [FUNCTION], [ORGANIZATION], [DATA], [PERIOD], [METRIC]

**Prompt:**

```text
Act as an experienced [FUNCTION] analyst at [ORGANIZATION]. Using [DATA] for [PERIOD], diagnose what is driving the change in [METRIC].
1. Compare the before and after periods across every dimension in the data, not only the obvious one, and show the change by segment.
2. Name the leading driver, then remove it and report what change remains and where it sits. Repeat until what is left is small or has no clear home.
3. For any cause suggested by comments, interviews, or the request, say what pattern it would leave in the structured data and whether that pattern is there.
4. Separate confirmed drivers from suspected ones, give each a share of the total change (with an "unexplained" share), and note what data would confirm the suspected ones.
```

**Good / Red flag:** Good: segment-level drivers with evidence labels, a remainder check after the leading driver, and shares that add to the total. Red flag: one uniform cause for everyone, or the diagnosis stops at the first driver found.


---

## AV-12 · Climb the ladder: project scenarios  
**Depth:** Extended · **Pairs with:** AV-13

**When to use:** Level 3 – shows leadership what happens if nothing changes.

**Placeholders:** [FUNCTION], [METRIC], [HORIZON], [UNITS]

**Prompt:**

```text
Continue as the [FUNCTION] analyst. Project [METRIC] for the next [HORIZON] assuming nothing else changes. Give a low, expected, and high scenario, the assumptions behind each, and the business impact in [UNITS, e.g., revenue, customers]. Flag which assumption the projection is most sensitive to.
```

**Good / Red flag:** Good: stated assumptions and a sensitivity callout. Red flag: precise single-number forecast from thin data.


---

## AV-13 · Climb the ladder: recommend actions  
**Depth:** Extended · **Pairs with:** CO-01, CO-04

**When to use:** Level 4 – turns diagnosis into options; follow with a human check for outside events AI can't see.

**Placeholders:** [FUNCTION], [METRIC], [HORIZON], [FORMAT]

**Prompt:**

```text
Acting as an advisor in [FUNCTION] and finance, recommend actions to improve [METRIC] over the next [HORIZON]. For each: expected impact, cost/effort, owner, how we'd measure it, and which verified finding it rests on. Rank them. Then list outside events (market, competitor, weather, policy, system changes) I should check with colleagues before recommending any of these. Ask me any questions you need first. Output as [FORMAT].
```

**Good / Red flag:** Good: each action traces to a finding. Red flag: actions that rely on unverified causes.


---

## AV-14 · Confidence triage of findings  
**Depth:** Extended · **Pairs with:** INT-04, CO-05

**When to use:** Final gate – sort findings into share now / test next / hold, so not everything waits for 100%.

**Placeholders:** [LIST], [ACTION]

**Prompt:**

```text
Here are my findings with their evidence: [LIST]. The action being considered is [ACTION]. Sort each finding into: (1) Share now – confirmed and sufficient for this action; (2) Share as hypothesis – directional, with the specific evidence that would confirm it; (3) Hold – too thin to mention yet. Explain each placement in one sentence.
```

**Good / Red flag:** Good: directional items are kept but labeled, not dropped. Red flag: a confirmed trend treated as proof of its cause.
