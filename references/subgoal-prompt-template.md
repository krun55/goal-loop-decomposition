# Sub-goal Prompt Template

Use this template to create the executable prompt for one sub-goal. Fill every bracket before handing it to goal mode or running it as the controller instruction.

```markdown
You are executing one sub-goal inside a larger goal loop.

Parent goal:
[one sentence]

Sub-goal ID and outcome:
[G# - concrete observable outcome]

Dependencies already accepted:
[IDs plus evidence summaries]

Allowed scope:
- Read:
- Write:
- Do not touch:

Context to load first:
- Files/docs:
- Relevant skills:
- Prior evidence:

Internal development loop:
1. Re-check the sub-goal outcome and acceptance gate.
2. If code behavior changes are required, use test-driven-development unless explicitly exempt.
3. Implement only this sub-goal.
4. Run the required verification commands/checks.
5. Inspect the resulting diff or artifact.
6. Report DONE, FAILED, NEEDS_CONTEXT, or BLOCKED.

Acceptance gate:
- [criterion 1 with command/evidence source]
- [criterion 2 with command/evidence source]
- [criterion 3 with command/evidence source]

Reporting format:
- Status:
- Files changed:
- Verification run:
- Evidence:
- Diff/artifact inspection:
- Risks or blockers:
```
