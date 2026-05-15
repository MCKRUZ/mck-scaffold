# L3 Spec-as-Source — Watch List

> Scaffolded into this project on {{scaffold_date}} by mck-scaffold v{{mck_scaffold_version}} because the interview detected **security-critical context** (Q1 = d, audit + security-sensitive domain) and you opted in to receive the L3 monitoring document.
>
> **This document does not change how the project is built today.** It exists so that *future you* — re-evaluating the methodology on or after {{re_eval_date}} — has a primed list of what changed in the L3 space and whether it now justifies migration.

## Current setup recap

This project uses **{{methodology_preset}}** with overlays {{methodology_overlays_or_none}}. That is the right setup for L2-Spec-Anchored work in mid-2026.

L3 (Spec-as-Source) is **not yet** the right call for production work in your context — it remains aspirational in 2026.

## What L3 means

Piskala's three-level taxonomy:

| Level | Promise | What it does |
|---|---|---|
| L1 Spec-First | Flexibility | Spec written before code; lives in feature branch; becomes historical after merge. |
| L2 Spec-Anchored *(your current level)* | Auditability + traceability | Spec lives the whole lifecycle; every change requires an addendum that propagates to design, code, tests. |
| **L3 Spec-as-Source** | **Guarantee by construction** | **Spec is the only edited artifact.** Code is generated. Files contain `// GENERATED FROM SPEC — DO NOT EDIT`. You change behavior by editing the spec and regenerating, not by editing code. |

The L3 promise: **zero spec drift, by design.** No human edits the code without going through the spec. Audit trails are not assembled retroactively — they exist because the only path to production was through the spec.

## Why we're not deploying L3 today

| Constraint | Status (as of {{scaffold_date}}) |
|---|---|
| Tessl Framework (Guy Podjarny, $125M funded) | **Closed beta.** No public install path. Spec Registry covers ~10k library specs (vendor figure). |
| Constitutional SDD (CSDD, Marri 2026, arXiv:2602.02584) | **Academic methodology.** Single empirical study (73% security defect reduction, banking microservices domain). No reference toolkit. |
| Production deployments outside OSS libraries and security-critical contexts | Essentially zero. |
| Ecosystem maturity | Tooling is sparse. Bus-factor risk on every available option. |

Wasowski 2026's stance: *"Spec-First for 80% of projects. Spec-Anchored for regulated ones. Spec-as-Source you watch, you don't deploy."* Honored here.

## What to monitor

Set a calendar reminder for **{{re_eval_date}}** to revisit each item.

### Tessl Framework

- **Does it exit closed beta?** Watch for a public install path (`tessl init` or equivalent without invite-only gating).
- **Is the Spec Registry still small or has it grown to npm scale?** Verify by sampling — pick 10 random libraries from your stack and check coverage. If <50% covered, the registry isn't ready.
- **Are there independent enterprise case studies?** Not vendor-published. Search for write-ups from engineers who weren't on the Tessl team. Three real production cases is the minimum signal.
- **Does the Eval Framework's `--prs` flag work as advertised?** It's supposed to drop specs that don't materially change generated code. Verify on a real project before betting on the registry's curation.

### CSDD (Constitutional SDD)

- **Has the empirical claim been replicated?** Marri 2026's 73% defect reduction was banking microservices. Watch for studies in other domains (healthcare, embedded, public sector). One study is interesting; three replications is durable.
- **Has a reference toolkit shipped?** Currently it's a paper. A real reference implementation (semgrep rule pack with SEC-NNN→CWE mappings + agent skills wired up) would be the maturity signal.

### Standards consolidation

- **MCP (Model Context Protocol)** — Anthropic Nov 2024, Linux Foundation Dec 2025, 97M monthly SDK downloads (Apr 2026). Watch for: deeper integration into Spec-as-Source tooling, e.g., `.spec.md` specs queryable via MCP servers.
- **AGENTS.md** — package.json-for-agents. 60k+ OSS projects as of early 2026; OpenAI Codex, Google Jules, Cursor, Factory all back it. Watch for: formal standardization (W3C, IETF, or Linux Foundation-hosted spec).

### Adjacent industry signals

- **AWS Kiro IDE** — Watch for: enterprise adoption normalizing approval-gate workflows as the default rather than the exception.
- **BMAD-METHOD / Qodo Audit Trail SaaS** — Watch for: a ready-made audit-trail-as-a-service product that you could plug in without committing to a specific framework. That would lower the cost of L2 → L3 migration significantly.

## Migration considerations (when L3 is ready)

When you re-evaluate and decide L3 is worth deploying, the cost is significant:

1. **All editable code becomes derived from spec.** Every existing file gets a `// GENERATED FROM SPEC — DO NOT EDIT` header. Direct edits become a constitutional violation.
2. **Spec authoring tooling must be production-ready.** EARS format is necessary but not sufficient — you need a spec editor (text editor + linter) that catches malformed specs *before* generation.
3. **Code review workflow flips.** You're not reviewing diffs of `.cs` / `.ts` / `.py` files — you're reviewing diffs of `.spec.md`. Reviewers need to be retrained.
4. **CI changes.** Build pipeline includes "regenerate code from spec; verify no diff against committed code." Drift detection becomes a CI step.
5. **Spec Registry dependence.** If you use one (Tessl-style), you're now coupled to its availability and curation. Treat it as a critical-path dependency.
6. **Variance ADRs become higher-stakes.** A documented variance from spec means a chunk of code that *isn't* generated. The audit trail must explain why.

Budget 1–3 quarters for migration of a non-trivial project. Don't try to migrate mid-development; bracket migration to a stable release cycle.

## What this document does NOT do

- It does not configure anything in this project. Settings, hooks, and conventions all come from `{{methodology_preset}}` and overlays.
- It is not a roadmap commitment. mck-scaffold doesn't promise to add L3 presets in any specific timeframe — that depends on the ecosystem maturing.
- It is not a substitute for re-running `/mck-scaffold:init` on the re-evaluation date. The interview routing may have changed by then; trust the re-run, not this static doc.

## Reference

- [Piskala 2026 — SDD rigor taxonomy (arXiv:2602.00180)](https://arxiv.org/abs/2602.00180)
- [Marri 2026 — Constitutional SDD (arXiv:2602.02584)](https://arxiv.org/abs/2602.02584)
- [Tessl Framework (closed beta)](https://tessl.io/)
- [Wasowski 2026 — Comparing 15 SDD Frameworks (Medium)](https://medium.com/)
- mck-scaffold repo — [`mck-scaffold/templates/methodologies/L3-watch/README.md`](https://github.com/MCKRUZ/mck-scaffold)
