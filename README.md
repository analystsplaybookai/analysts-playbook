# The Analyst's Playbook — Claude Plugin Marketplace (private)

*Move faster. Stand behind every number.*

This private repository is a Claude plugin marketplace. It holds one plugin,
**analysts-playbook**, an interview-driven harness that takes an analyst from a stakeholder
question to a verified, stakeholder-ready brief, while keeping AI costs and run logs in a
separate analyst-only folder.

## Install (for licensed users)

You need access to this private repository on GitHub and Claude Code (terminal, desktop app
Code tab, or VS Code) signed in on your machine.

1. Make sure git on your machine can reach the repo (for example, run `gh auth login` once).
2. In a Claude Code session, add the marketplace:
   ```
   /plugin marketplace add analystsplaybookai/analysts-playbook
   ```
3. Install the plugin:
   ```
   /plugin install analysts-playbook@analysts-playbook
   ```
4. Start a project:
   ```
   /analysts-playbook:start-project
   ```

Update later with `/plugin` → **Marketplaces** → **analysts-playbook** → **Update marketplace**.

## What the plugin does

| Skill | Stage | What it does |
|---|---|---|
| `start-project` | 0 Setup | Interviews you for every project variable once, builds the project folder, writes the reviewer stance and context brief, and plans cost. |
| `frame-question` | 1 Frame | De-biases the question, checks the premise, scopes the first pass, sets the evidence bar. |
| `prepare-data` | 2 Prepare and govern | Sensitivity and regulatory check, coverage check, fair-comparison check (paired per unit), missing-data owners, data-quality log. |
| `verify-findings` | 3 Analyze and verify | Diagnose, surface assumptions, reliability sweep, evidence labels, stress and pressure tests, scenarios, actions, confidence triage. |
| `deliver-brief` | 4 Communicate and operationalize | Scope-matched recommendation, working hypothesis, one-page pre-read, brief audit, anticipated questions. |
| `stakeholder-package` | 5 Stakeholder package (optional) | Slide deck, email draft and voiceover video from the approved brief, with email/slide reconciliation, a required video review, and an analyst-only prompts-and-replies review. Nothing reaches the client folder until you approve it. |
| `cost-and-log` | Throughout | METER cost plan before work, a log row for every prompt run, and a cost audit at close. |

Every project folder looks like this:

```
<client>_<project>_<date>/
  01_deliverables/    share these
  _analyst-only/      never share: profile, logs, raw responses, scorecard, cost plan and audit
```

See `examples/sample-project/` for a complete fictional run.

## Repository layout

```
.claude-plugin/marketplace.json        marketplace catalog
plugins/analysts-playbook/             the plugin
  .claude-plugin/plugin.json
  skills/<skill>/SKILL.md              instructions Claude follows
  skills/<skill>/references/           prompt library and supporting detail
  skills/start-project/templates/      project file templates
examples/sample-project/               fictional worked example
```

## Maintaining (for the owner)

- **Edit prompts** in `plugins/analysts-playbook/skills/<stage>/references/prompts.md`.
  Keep the IDs stable; the logs reference them.
- **Bump versions** in both `plugin.json` and `marketplace.json` for each release so users
  see the update.
- **Validate** before pushing: `claude plugin validate .`
- **Test locally** without installing: `claude --plugin-dir ./plugins/analysts-playbook`

© The Analyst's Playbook. Licensed for use by paying customers only. Do not redistribute.
