# L1-lean

> The minimum-viable Spec-First preset. Plan Mode + lean CLAUDE.md. No upstream framework installed.

## One-liner

Spec-First with no ceremony. Plan, then implement. Useful for personal scripts, throwaways, short experiments.

## When to pick this

Resolved by routing when:
- `Q1 = a` (no audit risk)
- `Q2 = greenfield`
- `Q4 = off` (TDD off / per-feature)

Or any time the user explicitly wants the lightest possible structure.

## What this preset contributes

- `CLAUDE.append.md` — adds a "Plan Mode is enough" section to the target's `CLAUDE.md`
- `settings.merge.json` — adds a single hook reminder at session start

## Upstream install commands

None. This preset deliberately ships zero external dependencies.

## Honest caveats

- **You will skip Plan Mode.** Lean discipline only works if the user holds the line. If you've ever caught yourself one-shotting a "quick fix" that turned into a 2-hour debug session, escalate to L1-spec-kit or L1-superpowers.
- **No artifact trail.** When a future you (or a colleague) asks "why was this built this way?" — the answer is "we didn't write it down." That's fine for throwaways, painful for anything that lives.
