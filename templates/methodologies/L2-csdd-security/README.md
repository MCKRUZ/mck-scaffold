# L2-csdd-security (overlay)

> Constitutional security overlay. Appends SEC-NNN clauses (with CWE references) to the primary L2 preset's constitution. Empirically 73% security defect reduction in a banking microservices study (Marri 2026, single source).

## One-liner

Security-by-construction overlay. Rules don't just *advise* the agent; constitution-enforcement rejects implementations that violate them. Composes on top of L2-musubi (not L2-openspec-brownfield).

## When to pick this

Resolved by routing as an **overlay** when:
- `Q1 ≥ c` (always audited or security-critical)

Always composed with `L2-musubi`. Never standalone.

## What this overlay contributes

- `constitution.append.md` — SEC-NNN clauses appended to `docs/constitution.md` (created by MUSUBI)
- `CLAUDE.append.md` — security-first reminder block appended to target's `CLAUDE.md`
- `settings.merge.json` — OWASP scan hook on commit, secrets-scan pre-commit hook

## What CSDD adds to MUSUBI

| Layer | MUSUBI provides | CSDD overlay adds |
|---|---|---|
| Constitution | 9 generic articles (Articles III, IV, V blocking) | SEC-NNN security clauses with CWE/MITRE-Top-25 references |
| Skills | `@security-auditor` (OWASP-aware) | Hardened hook config so the auditor actually runs |
| Traceability matrix | REQ→Design→Code→Test | + SEC→Code mapping (Compliance Traceability Matrix) |
| Static analysis | Optional | Required (semgrep / equivalent) on every commit |

## Honest caveats

- **The 73% number is one study.** Marri 2026, banking microservices. Don't quote it to a CISO as if it's industry-standard. It's a directional signal of "constraint-based security gates work better than after-the-fact audit," not a guaranteed reduction for your context.
- **You'll spend the first week tuning SEC clauses.** The defaults shipped here are a starting set (CWE-89, CWE-79, CWE-798, CWE-502, CWE-310, CWE-22, CWE-352). Real projects need domain-specific additions (PCI for payments, HIPAA for health, etc.).
- **Static analysis tool not provided.** This overlay assumes you have semgrep, Snyk Code, or equivalent installed. The hooks reference them; you wire the actual binary.
- **Brownfield mismatch.** Doesn't compose with L2-openspec-brownfield. For brownfield + security-critical, migrate to MUSUBI proper first (accepting the migration cost) and then apply CSDD overlay.
- **Re-eval the SEC clauses regularly.** CWE / MITRE Top 25 / OWASP Top 10 shift year over year. Set a calendar reminder for the re-evaluation date in `docs/decisions/METHODOLOGY.md`.
