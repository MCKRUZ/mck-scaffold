# L2-openspec-brownfield

> Spec-Anchored, brownfield-first. Delta specs (ADDED / MODIFIED / REMOVED) instead of full re-specification. Pairs with Shotgun CLI for code-graph indexing of the existing codebase.

## One-liner

The right preset when you're adding to a legacy codebase. Delta specs match the nature of brownfield work — you don't re-spec everything, you spec the *change*. OpenSpec is the lightest L2 framework.

## When to pick this

Resolved by routing when:
- `Q2 = brownfield` (any other answers)

Brownfield is the single most discriminating answer. Greenfield routing doesn't reach this preset.

## What this preset contributes

- `CLAUDE.append.md` — delta-spec workflow, Shotgun-first reminder
- `settings.merge.json` — empty (OpenSpec init handles directory creation)

## Upstream install commands

Run BEFORE `openspec init` so the code-graph is built first:

```
uvx shotgun plan "<short description of what you're adding>"
```

Then:

```
openspec init
```

Both triggered by the scaffolder in Phase 7.

Shotgun produces a code-graph index of the existing codebase that subsequent OpenSpec commands consume. Skipping Shotgun and going straight to `openspec init` works, but the agent will spend tokens re-discovering code structure that Shotgun already indexed.

## What OpenSpec gives you

- `openspec/` directory at the project root
- Delta-spec templates (ADDED / MODIFIED / REMOVED format)
- Spec-anchored discipline without full re-specification of legacy code
- The lightest L2 framework — auditability without MUSUBI's full ceremony

## Honest caveats

- **Code-graph indexing takes time.** First Shotgun run on a large codebase can take 5–30 minutes. Worth it; surprises new users.
- **Lighter than MUSUBI.** No EARS-format enforcement, no @traceability-auditor with 100% blocking. If you need full audit trail, escalate to L2-musubi (and run it against the existing code, accepting the migration cost).
- **One-maintainer project.** Bus-factor risk. Have a fallback plan.
- **Not for greenfield.** If you start a new project with OpenSpec, you'll be doing delta-of-empty for every spec. Use L1 or full L2-musubi for greenfield.
- **No security overlay.** L2-csdd-security pairs with L2-musubi, not with L2-openspec-brownfield. If you have brownfield + security-critical context, talk to the user about which constraint dominates.

## Composability

Does NOT compose with L2-csdd-security (CSDD overlay assumes MUSUBI's constitution.md format). For brownfield security-critical work, the recommendation is to migrate the project to MUSUBI proper rather than overlay CSDD on OpenSpec.
