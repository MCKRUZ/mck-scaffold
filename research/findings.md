# research/findings.md

> Consolidated narrative of the current ecosystem state. Synthesized from the 4 research streams (see `streams/`). Last refreshed: **2026-05-15** (baseline).

This file is the human-readable view of the catalog. Structured facts for routing live in `streams/*.md`. Deltas over time live in `changelog.md`.

## The shape of the landscape

Per Piskala 2026's three-level taxonomy:

- **L1 Spec-First** — flexibility, ~80% of projects. The crowded majority.
- **L2 Spec-Anchored** — auditability + traceability, regulated work.
- **L3 Spec-as-Source** — guarantee by construction, aspirational in 2026 — watch, don't deploy.

Per Wasowski 2026's four decision contexts:

- **A** Solo / small greenfield → L1
- **B** Team 10+ with compliance → L2 + CSDD overlay
- **C** Brownfield → L2-OpenSpec + Shotgun CLI
- **D** Security-critical → L2-MUSUBI + CSDD + OWASP overlay

These pairings (taxonomy + contexts) drive mck-scaffold's routing.

## Adoption is bimodal — popularity ≠ rigor

Two clusters dominate:

- **High-popularity, lower-rigor:** Superpowers (~166k stars), Spec-Kit (~90k). Designed for adoption; rigor is opt-in.
- **High-rigor, lower-popularity:** MUSUBI (28 stars), CSDD (academic). Designed for auditability; adoption requires the right context.

Choosing the wrong end of this spectrum is the #1 dysfunction in real deployments — Wasowski's central thesis. mck-scaffold's routing exists specifically to prevent this.

## Cross-source consensus tactics (high-confidence)

These appeared in 3+ unrelated sources during the baseline research:

1. **Plan Mode before any non-trivial code.** Universal.
2. **One task per session, `/clear` at ~60% context.** Context rot is real, documented in Anthropic's own engineering essays.
3. **Spec/PRD as durable file artifact.** Fix the file, not the chat.
4. **Lean CLAUDE.md (<150 lines, ~60 achievable).** Instruction-following degrades past ~200 total.
5. **Hooks for deterministic, CLAUDE.md for advisory.** If Claude has ever ignored a rule, it belongs in a hook.
6. **Subagents are for context isolation first, parallelism second.** Anthropic's multi-agent research system beat single-agent by 90% primarily because separate context windows aggregate more total tokens.
7. **Auto-delegation to subagents doesn't reliably work.** Must list explicitly in CLAUDE.md.
8. **Independent verification subagent for TDD.** Prevents overfitting to self-written tests.
9. **Press Escape early when Claude drifts.**
10. **Observability is mandatory at multi-agent scale.**

## Critical counter-signals (don't ignore)

- **Tool invocation + command execution = 62.6% of bugs** in a 3.8k-bug empirical study (arXiv 2603.20847). The model is fine; the harness is where failures live. Implication: invest in tool-invocation correctness, not elaborate agent choreography.
- **Multi-agent ~15x token cost** (Anthropic's own admission). Poor fit for tightly-interdependent coding tasks.
- **BMAD-METHOD is "a trap for two-person startups"** (Reddit consensus). Process multiplier, not creator.
- **Spec-Kit ~10x first-cycle overhead** (single measurement; directional). The artifact trail is real but costs real time.
- **HN top comment on Superpowers: "stripped it back to 30%"** — it's opinionated; that's a feature for serious work, friction for one-line fixes.

## Active drift watch

| Framework | Likely shift in next 6 months |
|---|---|
| Tessl | Exits closed beta? → may justify L3 productive scaffolding for security-critical contexts |
| Spec-Kit | v0.10.0 will require explicit opt-in for git extension; `--ai` flag removed |
| CSDD | Independent empirical replication of 73% defect-reduction claim? |
| AGENTS.md | Formal standardization (W3C / IETF / Linux Foundation)? Currently 60k+ projects + OpenAI / Google / Cursor / Factory backing. |
| MCP | Deeper integration into SDD tooling (97M monthly SDK downloads as of Apr 2026) |

These get re-checked every refresh. The `last-run.json` `known_drifts_since_last_run` field captures fresh items.

## Where this synthesis came from

- **GitHub stream:** repo survey + star-counting + install-command extraction; supplemented by Anthropic's example-plugin + Superpowers + context-mode marketplace manifests installed locally
- **Reddit stream:** aggregator sources (Morph LLM, AI Tool Discovery, HumanLayer, ClaudeKit) because primary Reddit MCP was down at baseline — flagged for next refresh to re-attempt direct access
- **YouTube stream:** Anthropic engineers (Boris Cherny, Cal Rueb), indie creators (IndyDevDan, Cole Medin, Net Ninja, alexop.dev, CampusX), BMAD masterclass author Brian
- **Web/blogs stream:** Anthropic engineering articles (Building Effective Agents, multi-agent research system, harness design, skills launch), Armin Ronacher's contrarian essays, Simon Willison on Agentic Engineering Patterns, Hamel Husain on Evals Skills, plus Shopify / Vercel / Sourcegraph engineering writeups, plus Wasowski's 15-framework Medium article and the Piskala/Marri arXiv papers
