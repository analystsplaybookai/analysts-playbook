# analysts-playbook (plugin)

Install: `/plugin install analysts-playbook --marketplace analystsplaybookai/analysts-playbook`

An AI harness for data analysts. Run `/analysts-playbook:start-project` to begin; each
stage recommends the next.

**Flow:** Setup → Frame → Prepare and govern → Analyze and verify → Communicate and operationalize,
with cost planning at the start and a cost audit at the end. An optional Stakeholder package
stage turns the approved brief into a deck, email draft, and voiceover video.

**Depth:** Core (11 essential prompts, lowest cost) or Extended (all 29). Chosen in the
interview; any single prompt can be added or skipped.

**Separation:** client-ready files go to `01_deliverables/`; costs, logs, raw AI responses,
and verification notes go to `_analyst-only/`. Deliverables are screened so they never
contain costs, prompt IDs, or tool references.

**Guardrails:** every stage checks the regulatory and policy constraints captured in the
interview (HIPAA, GDPR/CCPA, financial rules, internal policy) before touching data.

**Costs** are estimates using the METER levers (Model tier, Extract size, Turns,
Errors/redos, Reply length), not billing records.

Support: support@analystsplaybook.ai
