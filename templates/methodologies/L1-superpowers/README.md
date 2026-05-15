# L1-superpowers

> Spec-First with obra/Superpowers (~166k stars, Apr 2026). HARD-GATE in brainstorming SKILL.md. Mandatory TDD enforced by real test output. The most popular SDD framework of 2026.

## One-liner

Spec-First with an iron-law TDD gate. The agent literally cannot write production code without an approved design and a failing test first. Discipline-enforcement-by-construction.

## When to pick this

Resolved by routing when:
- `Q1 = a` (no audit risk) **and** `Q4 = mand` (TDD mandatory)
- Or solo/small team that wants the cheapest possible quality gate

User-facing rationale: "you trust yourself to skip steps; let the harness block you."

## What this preset contributes

- `CLAUDE.append.md` — short reminder pointing at `/superpowers:brainstorm`. Superpowers IS the content; we don't duplicate it.
- `settings.merge.json` — empty (Superpowers ships its own skills via plugin install)

## Upstream install commands

```
/plugin install superpowers@claude-plugins-official
/plugin reload
```

Triggered by the scaffolder in Phase 7. Superpowers itself populates `~/.claude/plugins/superpowers/` with the skill bundle.

## What Superpowers gives you

- **brainstorming/SKILL.md** with HARD-GATE: "Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until you have presented a design and the user has approved it."
- **writing-plans/SKILL.md** — produces hyper-detailed plans (complete runnable code per task, not pseudocode)
- **using-git-worktrees/SKILL.md** — isolates the work
- **test-driven-development/SKILL.md** — enforces RED-GREEN-REFACTOR by checking actual test output
- **subagent-driven-development/SKILL.md** — fresh subagent per task with 2-stage review (spec compliance + code quality)
- **systematic-debugging/SKILL.md** — 4-phase root-cause process

## Honest caveats

- **It is opinionated.** The most-upvoted HN comment on Superpowers: "I ended up stripping it back to get something useful. Kept maybe 30%." That's the cost of HARD-GATE.
- **Overkill for throwaways.** A 20-minute bug fix doesn't want a brainstorming Socratic dialogue. Use `L1-lean` for those.
- **Claude Code only.** No first-class Cursor / Codex / Gemini support (though they can read the skills if pointed at them manually).
- **No traceability.** No REQ-XXX-NNN identifiers, no matrix. If an auditor will ask, escalate to `L2-musubi`.
- **No brownfield support.** No delta-spec concept. If you're adding to an existing 5-year codebase, the brainstorming flow will fight you. Use `L2-openspec-brownfield` instead.
- **Star count is fast-moving.** Reports range from ~93k to ~166k in 4–7 months 2025–2026. Adoption is real but treat any specific number as approximate.
