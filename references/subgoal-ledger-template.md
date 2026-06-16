# Sub-goal Ledger Template

Use this template when the parent goal needs more than two sub-goals or when dependencies matter.

```markdown
# Goal Loop Ledger

Parent goal:

Success criteria:

Assumptions:

Non-goals:

Global constraints:

## Dependency graph

G1 -> G3
G2 -> G3
G3 -> G4

## Sub-goals

| ID | Status | Depends on | Outcome | Scope | Acceptance gate | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| G1 | ready | - |  |  |  |  |
| G2 | pending | - |  |  |  |  |
| G3 | pending | G1, G2 |  |  |  |  |

## Status Values

- `pending`: waiting for dependencies or not yet selected.
- `ready`: dependencies are accepted and the sub-goal can run.
- `in_progress`: currently being executed.
- `accepted`: passed every acceptance gate with fresh evidence.
- `failed`: attempted but gate failed and a retry is planned.
- `blocked`: cannot continue without user input or an external change.

## Loop Notes

Record each attempt as:

- Sub-goal:
- Prompt used:
- Commands/checks run:
- Result:
- Diff/artifact inspected:
- Decision:
- Next ready set:
```
