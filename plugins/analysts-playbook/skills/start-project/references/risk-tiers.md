# Risk Tiers: Two-Way Scoring

Every project gets a risk tier of **low**, **medium**, or **high** before analysis starts.
The analyst and the AI score it separately; a mismatch triggers more questions. The tier
sets the controls in conventions §10.

## The intake questions

Ask these first (AskUserQuestion, one round). Each maps to a scoring factor.

| Factor | Question | Low | Medium | High |
|---|---|---|---|---|
| Impact | What happens if the answer is wrong? | Minor rework | Wasted spend or a delayed decision | Money, people, patients, or customers harmed |
| Reversibility | Can the decision it feeds be undone? | Easily | With cost or effort | Not, or not quickly |
| Data sensitivity | How sensitive is the data? | Public or aggregated | Internal or confidential | Personal or regulated (see the `regulatory` list) |
| Audience | Who acts on it? | The analyst or their team | Managers or a department | Executives, a board, customers, or regulators |
| Wrong data in | Could bad, stale, or sensitive input get into the analysis or the AI tool? | Unlikely; one trusted source | Possible; several sources or manual steps | Likely, or sensitive data could reach the AI tool |
| Decision trigger | What decision will the number trigger, and by when? | No decision, or weeks away | A planned decision with time to check | A decision or deadline within days |

The decision-trigger question usually moves the tier more than any other: a number that
sets off a renewal, a firing, or a price change by Friday is high even when the data is tame.

## Scoring steps

1. Ask the intake questions above and record the answers.
2. The analyst picks low, medium, or high for the overall project **before** seeing the AI's
   score. Ask this as its own question.
3. The AI scores each factor, then gives its overall tier (the highest factor, unless a
   one-line reason says why not) with a one-line reason.
4. If the two overall tiers match, save it and move on. If not, name the factor(s) behind the
   gap and ask one or two follow-up questions aimed at those factors. Both re-score. If they
   still differ, the **higher tier** is the default unless a person with authority over the
   work decides otherwise; record who.
5. Save both scores, the factor scores, any follow-up questions, and the final tier to the
   profile's `risk` block and as one row in `prompt-log.csv` (ID `RISK-01`, stage "0 Setup").

**Fluency cap (not yet active).** The final tier should not exceed the analyst's fluency
tier, so a newer analyst never takes a high-risk project alone. Fluency tiers are not
defined yet; until they are, record "fluency cap: not set" in the profile.

## Sample projects by tier

**Low**
- Summarize last month's support-ticket volume by category for the team's own planning.
- Explore whether website sessions differ by weekday, to decide when to post content.

**Medium**
- Explain why a regional sales metric dropped last quarter, for a director's quarterly review.
- Compare two suppliers' on-time delivery to inform next year's contract talks.

**High**
- Decide whether to renew a five-figure vendor contract due in two weeks, using patient
  appointment data.
- Recommend which locations to close, for a board vote, using staff and payroll data.
