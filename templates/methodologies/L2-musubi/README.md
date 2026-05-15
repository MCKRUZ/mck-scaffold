# L2-musubi

> Spec-Anchored with MUSUBI. EARS-formatted requirements + C4 architecture diagrams + ADR + REQ→Design→Code→Test traceability matrix + 9-article constitution with merge-blocking enforcement.

## One-liner

The strongest formal-rigor preset that is actually installable in 2026. 28 GitHub stars; that's a signal it's niche, **not** a signal it's bad. If an auditor will ask about REQ-to-Test traceability, MUSUBI pays for itself before they finish the question.

## When to pick this

Resolved by routing when:
- `Q1 = b` (sometimes audited) **and** `Q4 = mand`
- Or `Q1 ≥ c` (always audited or security-critical)
- Or `Q3 = team10+` **and** `Q1 ≥ b`

User-facing rationale: "someone will ask for traceability evidence; you want that evidence to exist."

## What this preset contributes

- `CLAUDE.append.md` — MUSUBI orchestration cues, EARS reminder, constitution-as-contract framing
- `settings.merge.json` — pre-commit traceability check stub, audit log hook stub

## Upstream install commands

```
npx musubi-sdd@latest init
```

Triggered by the scaffolder in Phase 7. MUSUBI populates the target project with:
- `.claude/skills/` — 25–31 skills (`@orchestrator`, `@requirements-analyst`, `@system-architect`, `@test-engineer`, `@traceability-auditor`, `@constitution-enforcer`, `@security-auditor`, `@change-impact-analyzer`)
- `docs/constitution.md` — 9 articles (Articles III, IV, V are blocking)
- `docs/specs/` — EARS-format requirement templates with REQ-DOMAIN-NNN identifiers
- `docs/design/` — C4 + ADR templates

## EARS format quick reference

Easy Approach to Requirements Syntax (Rolls-Royce origin). Requires SHALL/MUST; forbids should/may/could/might/would. Five patterns:

| Pattern | Form |
|---|---|
| Ubiquitous | "The system SHALL X." |
| Event-Driven | "WHEN X, the system SHALL Y." |
| State-Driven | "WHILE X, the system SHALL Y." |
| Optional Feature | "WHERE X is available, the system SHALL Y." |
| Unwanted Behaviour | "IF X, THEN the system SHALL Y." |

Every requirement gets an identifier (`REQ-AUTH-001`, `REQ-BILL-042`, etc.). The `@traceability-auditor` skill blocks merge if coverage drops below 100%.

## Honest caveats

- **EARS has a real learning curve.** SHALL/MUST discipline is harder than it sounds. Expect a sprint or two of friction before it becomes natural.
- **Process overhead is medium-high.** The constitution enforcement, traceability checks, and addendum requirements add real time. Worth it for the auditability promise; expensive for projects that don't need it.
- **28 stars.** That's a community-size signal, not a quality signal. The team behind MUSUBI is small. If they pause maintenance, you may need to fork.
- **Wrong tool for solo throwaways.** If no auditor will ever ask, L1-spec-kit or L1-superpowers are the right tool. MUSUBI's overhead only pays back when traceability is demanded.

## Composability

`L2-csdd-security` overlay composes on top of MUSUBI when `Q1 ≥ c`. It appends CSDD security clauses (SEC-NNN with CWE references) to MUSUBI's `docs/constitution.md` and adds OWASP scan hooks to settings.
