# L3-watch (overlay)

> Informational-only overlay. Surfaced when the interview detects security-critical context (`Q1 = d`) AND the user explicitly opts in (`Q8 = yes`). Writes a single doc to the target project's `docs/horizon/` describing what to monitor in the L3 Spec-as-Source space.

## One-liner

L3 productive scaffolding is not viable in 2026. This overlay tells you *why* and *what to watch for* so you're not blindsided when it matures.

## When to compose

- `Q1 = d` (security-critical) **and** `Q8 = yes`
- Always composed alongside `L2-musubi` + `L2-csdd-security`. Never standalone — there's nothing to install.

## What this overlay contributes

- `horizon/L3-WATCH.md` — written to target project as `docs/horizon/L3-WATCH.md`

That's it. No CLAUDE.append.md. No settings.merge.json. No upstream install. Purely informational.

## Why "watch, not deploy"

- **Tessl**: closed beta (Apr 2026). No public install path. $125M funded but commercially restricted.
- **CSDD**: academic methodology (Marri 2026, arXiv:2602.02584). A paper with one empirical study (73% security defect reduction in banking microservices). No reference toolkit beyond the paper.
- **Spec Registry**: ~10k library specs claimed (vendor figure, unverified). Not at npm scale.
- **Production deployments outside OSS libraries and security-critical contexts**: essentially zero as of mid-2026.

Wasowski 2026's stance — "Spec-First for 80% of projects. Spec-Anchored for regulated ones. Spec-as-Source you watch, you don't deploy" — survives scrutiny. This overlay codifies that stance for the user's future-self.

## Re-eval cadence

The L3-WATCH.md template suggests revisiting in 6–12 months. By 2027 Q1 expect:
- Whether Tessl exits closed beta
- Whether `.spec.md` with `// GENERATED FROM SPEC` annotations gains non-Tessl adoption
- Whether AWS Kiro IDE or equivalent normalizes approval-gate workflows
- Whether MCP + AGENTS.md consolidation drives standardization
