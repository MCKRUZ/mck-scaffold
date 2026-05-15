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

- `CLAUDE.append.md` — adds Spec-Kit invocation cues to target's `CLAUDE.md` (uses modern `/speckit-X` hyphen format)
- `settings.merge.json` — adds nothing yet (Spec-Kit's `specify init` populates `.specify/` directly)

## Upstream install commands

**Windows:**

```powershell
$env:PYTHONIOENCODING = "utf-8"
$env:PYTHONUTF8 = "1"
uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude --force --script ps
```

**macOS / Linux:**

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude --force --script sh
```

Phase 8 of init/SKILL.md **auto-executes** these (Phase 11+ contract — not optional manual step). Routing.md's "Meta-installer triggers per preset" section is the canonical reference.

Spec-Kit will populate the target project with:
- `.specify/memory/constitution.md` — 9 articles
- `.specify/templates/spec-template.md`, `plan-template.md`, `tasks-template.md`
- `.claude/skills/` — 5 core skills (`speckit-constitution`, `speckit-specify`, `speckit-plan`, `speckit-tasks`, `speckit-implement`) plus 3 enhancement skills (`speckit-clarify`, `speckit-analyze`, `speckit-checklist`)
- Auto-`git init` (Spec-Kit's default; will require explicit opt-in starting in Spec-Kit v0.10.0)

## Drift notes (verified 2026-05-15)

Spec-Kit shifted between when Wasowski 2026 wrote the comparison article and current state:

| What's reported in older docs | Current reality |
|---|---|
| `--ai claude` flag | `--ai` is deprecated; use `--integration claude` |
| Slash commands: `/speckit.constitution` (dot) | `/speckit-constitution` (hyphen) |
| Commands live in `.claude/commands/speckit.*.md` | Skills live in `.claude/skills/` |
| Manual `git init` needed after | Spec-Kit auto-`git init`s (changing in v0.10.0) |

The mck-scaffold research-refresh pipeline (Phase 12) is designed to catch this kind of drift automatically.

## Honest caveats

- **First-cycle overhead is real.** A single Nov-2025 measurement showed ~33.5 min agent time on Spec-Kit vs ~8 min on a minimal baseline — roughly **10x slowdown** on the first cycle. That's an acceptance cost, not a defect.
- **No TDD enforcement.** Spec-Kit's constitution Article III says "Test-First Imperative" but it's advisory — the agent can route around it. If you need a hard gate, escalate to `L1-superpowers`.
- **PR queue may be stale.** Reports as of early 2026 suggest the upstream maintainer is slow on PRs and there's no community channel. Treat the constitution articles as the value; the tooling churn is secondary.
- **Bad fit for legacy.** The constitution will conflict with existing patterns in a 5-year codebase. For brownfield, route to `L2-openspec-brownfield` instead.
- **`.claude/` security advisory.** Spec-Kit's own post-install output recommends adding parts of `.claude/` to `.gitignore` to prevent agent credential leakage. The CLAUDE.append.md surfaces this; the target project's `.gitignore` should reflect it.
