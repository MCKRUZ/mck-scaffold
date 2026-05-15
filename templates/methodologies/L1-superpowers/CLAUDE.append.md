### L1-superpowers — methodology notes

This project uses **Superpowers** (obra/Superpowers, L1 Spec-First with HARD-GATE TDD enforcement).

**Workflow for any non-trivial feature:**

1. Type your intent. Superpowers' `brainstorming` skill auto-triggers and steps you through a Socratic dialogue (scope check, alternative approaches, design sections).
2. Approve the design. The HARD-GATE prevents any code being written before this step.
3. `writing-plans` skill takes over — produces a hyper-detailed plan (complete code per task, not pseudocode).
4. `executing-plans` / `subagent-driven-development` runs tasks with fresh-context subagents and 2-stage review.
5. Every task is RED-GREEN-REFACTOR: failing test first, watch it fail, write the minimal code, watch it pass, commit.

**Direct invocation** (if you want to start a flow yourself rather than waiting for auto-trigger):

- `/superpowers:brainstorm` — start a design dialogue
- `/superpowers:write-plan` — generate the plan from an approved design
- `/superpowers:execute-plan` — run the plan with subagents

**HARD-GATE rule** (verbatim from Superpowers):

> Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it.

If the workflow is too heavy for a one-line change, that's fine — small fixes don't need brainstorming. Use judgment. But for anything that touches more than a couple of files, run the full flow.

**TDD iron law** (verbatim):

> NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.

This is enforced by Superpowers checking real test output (e.g., "14 tests pass, 0 failing"), not by trusting the agent's declaration.
