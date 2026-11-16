# Variable Registry

Every placeholder used by the Playbook prompts, grouped by when it is collected.
`Key` is the field name in `project-profile.json` → `variables`.

## A. Project-level variables (asked once, in the start-project interview)

Ask in the rounds shown. Each round is one AskUserQuestion call (max 4 questions).
Offer the listed options; the analyst can type their own answer.

### Round 1: Who and why

| Key | Placeholder(s) | Question | Options to offer | Used by |
|---|---|---|---|---|
| organization | [ORGANIZATION] | Which organization or client is this analysis for? | (free text; offer folder name or prior profile if present) | SET-02, AV-11 |
| function | [FUNCTION] | Which function's lens should the analysis take? | Operations · Finance · Marketing · Sales · Customer service | SET-02, AV-11–13 |
| industry | [INDUSTRY] | What industry is the organization in? | Healthcare · Retail / e-commerce · Financial services · Professional services | DR-02 |
| decision | [DECISION] | What decision will this analysis inform? | (free text; suggest from the question if given) | INT-04, AV-14 |

### Round 2: The question

| Key | Placeholder(s) | Question | Options to offer | Used by |
|---|---|---|---|---|
| metric | [METRIC] | Which metric is at the center of the question? | (offer column names from the data if loaded) | INT-01, INT-03, AV-11–13, DR-03 |
| population | [POPULATION / SEGMENT DEFINITION] | Which population or segment is in scope? | All customers · A named segment · A region or site | INT-03 |
| time_window | [TIME WINDOW], [PERIOD] | What time window should the analysis cover? | Last full quarter · Last 12 months · Year to date | INT-01, INT-03, AV-11 |
| baseline | [BASELINE] | What should it be compared with? | Same period last year · Prior period · Target or budget | INT-03 |

### Round 3: Data and constraints

| Key | Placeholder(s) | Question | Options to offer | Used by |
|---|---|---|---|---|
| data_inventory | Data provided | Which files or tables are in scope, and what does one row represent? | (offer files found in the project folder) | SET-02, AV-11 |
| known_gaps | Known gaps | Which systems or data are NOT included? | CRM · Marketing · Support tickets · Finance | SET-02, DR-02 |
| regulatory | (guardrail) | Which rules or policies apply to this data? (multi-select) | HIPAA · GDPR / CCPA · Financial (GLBA, PCI) · Internal policy only | every stage |
| business_context | business context | In 2–3 sentences: the business, its customers, and why this matters now. | (free text) | SET-02 |

### Round 4: Output and audience

| Key | Placeholder(s) | Question | Options to offer | Used by |
|---|---|---|---|---|
| audience | [AUDIENCE] | Who reads the final brief? | Executive team · Department head · Board · Client sponsor | CO-01 |
| roles | [ROLES] | Which roles will question the findings in the meeting? | Finance · Operations · Sales leadership · IT / data owners | CO-03 |
| output_style | [FORMAT], format, tone | What format and tone should responses use? | Short bullets then a table, plain tone · Executive summary first · Detailed narrative | SET-02, AV-13 |
| depth | (tier) | How thorough should this run be? | Core (fastest, lowest cost) · Extended (adds stress tests, scenarios, meeting prep) | all |

### Round 5: Settings (optional; offer defaults, ask only if analyst wants to change)

| Key | Default | Meaning |
|---|---|---|
| evidence_tags | Confirmed / Probable / Speculative | Organization's confidence vocabulary |
| deliverable_header | value of `organization` | Header line on client deliverables (e.g., team name, or "THE ANALYST'S PLAYBOOK" for consulting engagements) |
| ai_tool | Claude | Logged in prompt-log.csv |
| model_tier | default | Which model tier the analyst plans to use; feeds the cost plan |
| show_prompts | false | Show each filled prompt before it runs |
| deliverable_format | markdown | markdown or docx |

## B. Run-level variables (resolved when the prompt runs)

These come from the analysis itself. Draft them from prior responses or the scorecard, then
ask the analyst to confirm or edit. Ask fresh only if nothing can be drafted.

| Placeholder | How to draft it | Used by |
|---|---|---|
| [AMOUNT], direction (rose/fell) | From the analyst's question or the first summary of the metric | INT-01 |
| [YOUR QUESTION] | The analyst's question in their own words | INT-02 |
| [FINDING] | The scorecard finding currently being tested | DR-02, AV-01, AV-04, AV-05, AV-09, CO-04 |
| [SEGMENT] | The segment dimension most likely to split the finding (from data columns) | AV-01 |
| [CLAIM] | The specific statement the analyst questioned | AV-06 |
| [CONCLUSION] | The analyst's working conclusion, stated with metric, segment, period | AV-07, AV-08, AV-09 |
| data coverage | What the data covers and does not (from profile data_inventory + known_gaps) | AV-08 |
| number of alternatives | Default 3 | AV-08 |
| [SHORT WINDOW], [LONGER WINDOW] | Short = the finding's window; longer = 2–4x as long, plus same window last year | AV-04 |
| [SUSPECTED CAUSE], [AFFECTED GROUP] | From the diagnosis or analyst's hypothesis | AV-05 |
| [CAUSE], [EFFECT], dates, [LOOKBACK PERIOD] | From the hypothesis; lookback default 6 months | AV-10 |
| [DATA] | profile data_inventory | AV-11 |
| [HORIZON] | Default next 2 quarters | AV-12, AV-13 |
| [UNITS] | Revenue, customers, cases, etc. — infer from metric, then confirm | AV-12 |
| [LIST] | All scorecard findings with evidence | AV-14 |
| [ACTION], scope, [COST] | The action being considered; ask for scope and rough cost/effort | AV-14, CO-04 |
| [GROUP A], [GROUP B] | The two groups the analyst wants to compare | DR-03 |
| [DATASET DESCRIPTION], [PURPOSE], [FIELD LIST] | From data_inventory, decision, and the data's column names | DR-04 |
| [PASTE BRIEF] | Contents of `01_deliverables/pre-read-brief.md` | CO-02 |
| [PASTE OR SUMMARIZE] | Brief plus scorecard summary | CO-03 |
| number (questions) | Default 5 | CO-03 |
| [EXPLANATION] | Leading hypothesis from the scorecard | CO-05 |
