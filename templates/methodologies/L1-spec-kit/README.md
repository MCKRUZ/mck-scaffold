# L1-spec-kit

> Spec-First with GitHub's Spec-Kit. Constitution + 5-command pipeline (specify → plan → tasks → implement). ~90k stars; multi-tool portable.

## One-liner

The most popular structured Spec-First preset, with the smallest opinion footprint. Good baseline when you want artifacts (constitution.md, spec.md, plan.md, tasks.md) without TDD enforcement.

## When to pick this

Resolved by routing when:
- `Q1 = a` (no audit risk) **and** `Q4 = enc` (TDD encouraged, not mandatory)
- Or `Q1 = a` **and** `Q3 = team10` (any TDD setting)
- Or `Q1 = b` (sometimes audited) **and** `Q4 ≠ mand`

User-facing rationale: "you want structure but not friction; auditor isn't asking."

## What this preset contributes

- `CLAUDE.append.md` — adds Spec-Kit invocation cues to target's `CLAUDE.md`
- `settings.merge.json` — adds nothing yet (Spec-Kit's `specify init` populates `.specify/` directly)

## Upstream install commands

```
specify init {{project_name}} --ai claude
```

This is triggered by the scaffolder in Phase 7 (meta-installer trigger logic), not by this preset directly.

Spec-Kit will populate the target project with:
- `.specify/memory/constitution.md` — 9 articles
- `.specify/templates/spec-template.md`, `plan-template.md`, `tasks-template.md`
- `.claude/commands/speckit.{constitution,specify,plan,tasks,implement}.md`

## Honest caveats

- **First-cycle overhead is real.** A single Nov-2025 measurement showed ~33.5 min agent time on Spec-Kit vs ~8 min on a minimal baseline — roughly **10x slowdown** on the first cycle. That's an acceptance cost, not a defect.
- **No TDD enforcement.** Spec-Kit's constitution Article III says "Test-First Imperative" but it's advisory — the agent can route around it. If you need a hard gate, escalate to `L1-superpowers`.
- **PR queue may be stale.** Reports as of early 2026 suggest the upstream maintainer is slow on PRs and there's no community channel. Treat the constitution articles as the value; the tooling churn is secondary.
- **Bad fit for legacy.** The constitution will conflict with existing patterns in a 5-year codebase. For brownfield, route to `L2-openspec-brownfield` instead.
