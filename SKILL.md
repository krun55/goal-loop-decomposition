---
name: goal-loop-decomposition
description: Use when a user gives a broad goal and wants explicit parent-goal tracking, dependent sub-goals, execution prompts, autonomous next-goal selection, or forced acceptance gates before continuing.
---

# Goal Loop Decomposition

## Overview

Turn a broad goal into a dependency-ordered loop of small sub-goals. Run one ready sub-goal at a time, enforce acceptance evidence, then select the next ready sub-goal until the parent goal is complete or truly blocked.

## Core Rules

- Keep one controller ledger for the parent goal. Do not rely on memory alone.
- Explicitly create or adopt a parent goal when goal-management tools are available and policy permits it. The parent goal is the system-tracked objective; the ledger is for sub-goals and dependency state.
- Do not create nested goals when the environment only allows one active goal. If a parent goal already exists and cannot be nested under, adopt it as the outer goal and manage all sub-goals in the ledger.
- In environments without goal-management tools, such as some Claude Code setups, run the loop directly from the ledger and record status there.
- Never accept a sub-goal without fresh verification evidence. Use `verification-before-completion` before marking any sub-goal accepted.
- Do not spawn subagents or separate threads unless the user or current tool policy explicitly allows delegation. If delegation is allowed, give each worker a disjoint write scope and still verify before acceptance.
- If a sub-goal fails its gate, fix and rerun the gate. If the same blocker repeats three times, mark that sub-goal `blocked` with evidence and choose another ready sub-goal if one exists.

## Workflow

1. **Capture the parent goal**
   - Restate the desired outcome, success criteria, non-goals, workspace boundaries, and known constraints.
   - If goal-management tools are available and there is no incompatible active goal, create the parent goal explicitly before executing sub-goals.
   - If explicit creation is skipped because nesting is impossible or tools are unavailable, record that fallback in the ledger.
   - Ask one blocking question only when decomposition would be risky without the answer. Otherwise make explicit assumptions and continue.

2. **Build the dependency graph**
   - Split work into atomic sub-goals with observable outcomes.
   - For each sub-goal, record dependencies, expected artifacts, allowed scope, acceptance gate, and verification command or evidence source.
   - Reject cycles. If a cycle appears, split or reorder the sub-goals before execution.
   - Use `references/subgoal-ledger-template.md` when a persistent ledger would help.

3. **Choose the next sub-goal**
   - Ready means every dependency is accepted and the sub-goal is not accepted or blocked.
   - Prefer the ready sub-goal that unblocks the most downstream work.
   - Break ties by highest risk first, then cheapest verification, then smallest write scope.
   - If nothing is ready and unfinished work remains, report the dependency/blocker instead of forcing progress.

4. **Generate the sub-goal prompt**
   - Use `references/subgoal-prompt-template.md`.
   - Include the parent goal, exact sub-goal outcome, dependencies already accepted, allowed write/read scope, acceptance gate, verification command, and reporting format.
   - Keep sub-goals inside the controller ledger unless the current environment explicitly supports nested goals. If nested goals are unsupported, run the sub-goal prompt as the current controller instruction.

5. **Run the internal development loop**
   - Load any applicable workflow skills for the sub-goal before work begins.
   - For code changes, use `test-driven-development` unless the user or repo explicitly exempts the change.
   - For bugs or unexpected failures, use `systematic-debugging`.
   - Keep edits scoped to the sub-goal. Do not opportunistically complete later sub-goals.
   - Run the acceptance gate and inspect the diff/artifacts before marking acceptance.

6. **Gate and update**
   - Mark `accepted` only when all acceptance criteria have matching fresh evidence.
   - Mark `blocked` only with the exact failed command, error, attempted fixes, and why the controller cannot proceed.
   - After acceptance, update downstream readiness and loop back to step 3.

7. **Finish the parent goal**
   - Run any parent-level final verification after all sub-goals are accepted.
   - Only then mark the created or adopted parent goal complete when a system-tracked parent goal exists and tool policy allows it.
   - Report the final ledger, evidence summary, and remaining risks. If any sub-goal is blocked, do not claim the parent goal is complete.

## Acceptance Gate

A sub-goal passes only when every item below is true:

- The stated artifact or behavior exists.
- Every acceptance criterion has direct evidence.
- Required tests, builds, linters, renders, or manual checks were freshly run.
- Verification output was read, not assumed.
- The diff/artifact inspection shows no unrelated or accidental changes.
- Any skipped verification is explicitly allowed by the sub-goal acceptance criteria.

## Output Shape

Keep controller updates compact:

```markdown
Parent goal: ...
Current sub-goal: G2 - ...
Ready set: G2, G4
Chosen next: G2 because it unlocks G5 and has the highest risk.
Gate: pending | accepted | failed | blocked
Evidence: command/output summary or artifact path
Next: ...
```
