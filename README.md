# mck-scaffold

> Interview-driven project scaffolder for Claude Code. Picks the right Spec-Driven Development methodology for your project — instead of installing the most popular one.

**Status:** v0.9.0 — functionally complete. v1.0 ships after [TEST_PLAN.md](./TEST_PLAN.md) dogfooding pass.

## What it does

You run `/mck-scaffold:init` in a project folder. It walks you through an 8-question interview, routes your answers through Piskala's three-level rigor taxonomy and Wasowski's four decision contexts, picks the right methodology preset (Superpowers, Spec-Kit, MUSUBI, OpenSpec, or a lean baseline), then scaffolds your project:

- `CLAUDE.md` — composed from shared base + methodology fragment + stack adapter
- `AGENTS.md` — cross-tool agent baseline (Codex / Cursor / Gemini / Copilot)
- `.claude/settings.json` — hook arrays + permissions per methodology and stack
- `docs/decisions/INTERVIEW.md` + `METHODOLOGY.md` — durable record of why this setup was chosen
- Triggers upstream framework installs as needed (`/plugin install superpowers@…`, `specify init`, `npx musubi-sdd@latest init`, `openspec init`, `uvx shotgun plan`)

The interview is the product. The templates are commodity.

## Why

The agentic-coding ecosystem (mid-2026) has 15+ SDD frameworks. Adoption is bimodal — Superpowers (~166k stars) and Spec-Kit (~90k) dominate popularity; MUSUBI (28 stars) and CSDD (academic) dominate formal rigor. Choosing the wrong end of this spectrum is the most common dysfunction in real deployments.

mck-scaffold makes the routing decision once, with full visibility into the tradeoffs, and produces a coherent setup with a durable decision trail.

## Quickstart

**From GitHub (canonical):**

```
/plugin marketplace add github:MCKRUZ/mck-scaffold
/plugin install mck-scaffold

cd /path/to/new-or-existing-project
claude
> /mck-scaffold:init
```

**From local checkout (for development on the plugin itself):**

```
/plugin marketplace add file:///C:/Users/RP325UG/OneDrive%20-%20EY/Documents/GitHub/MCKRUZ/mck-scaffold/
/plugin install mck-scaffold
```

For a *guidance-only* conversation (no file writes), describe what you want — the `methodology-picker` skill auto-triggers on phrases like "set up a new project," "pick a methodology," "what SDD framework should I use":

```
> I'm starting a new Angular API + .NET service for a regulated fintech client. What methodology should I use?
```

For a *dry run* of the scaffolder (interview only, no writes):

```
> /mck-scaffold:init --dry-run
```

## The interview, briefly

Eight questions. Smart pruning — brownfield skips Q3-Q4; non-security-critical context skips Q8.

| # | Question | Routes by |
|---|---|---|
| Q1 | Audit / compliance posture? | L1 vs L2 vs L2+security |
| Q2 | Greenfield or brownfield? | Brownfield → OpenSpec |
| Q3 | Team size? | Solo / small / 10+ |
| Q4 | TDD discipline? | Mandatory → Superpowers (L1) or MUSUBI Article III (L2) |
| Q5 | Stack? | Composes .NET / Angular / Unity / Python adapters |
| Q6 | MCP gateway available? | MCP vs CLI orientation |
| Q7 | Multi-agent appetite? | None / Limited (capped — no platforms in v1) |
| Q8 | L3-watch document? | Only asked if Q1 = security-critical |

Same answers always resolve to the same preset. Deterministic.

## Conceptual references

- **Piskala 2026** ([arXiv:2602.00180](https://arxiv.org/abs/2602.00180)) — three-level SDD rigor taxonomy (L1 Spec-First / L2 Spec-Anchored / L3 Spec-as-Source)
- **Wasowski 2026** (Medium) — *Comparing 15 SDD Frameworks*, four decision contexts
- **Marri 2026** ([arXiv:2602.02584](https://arxiv.org/abs/2602.02584)) — Constitutional SDD (security overlay)
- **Anthropic** — *Building Effective Agents*, *Effective Context Engineering*, *Best Practices for Claude Code*

## Repo layout

```
mck-scaffold/
├── .claude-plugin/plugin.json
├── README.md                       ← you are here
├── DESIGN.md                       ← full design and decisions
├── CHANGELOG.md                    ← phase-by-phase progress
├── TEST_PLAN.md                    ← dogfood procedure
├── skills/
│   ├── init/SKILL.md               ← /mck-scaffold:init — interview + scaffold + install
│   └── methodology-picker/SKILL.md ← conversational guidance (auto-trigger)
├── interview/
│   ├── questions.md                ← Q1-Q8 tree
│   └── routing.md                  ← answer-combination → preset
└── templates/
    ├── shared/                     ← base files (CLAUDE.md, AGENTS.md, settings, INTERVIEW.md, METHODOLOGY.md) + variable schema
    ├── methodologies/
    │   ├── L1-lean/                ← Plan Mode + lean CLAUDE.md
    │   ├── L1-spec-kit/            ← GitHub Spec-Kit (90k★)
    │   ├── L1-superpowers/         ← obra Superpowers (~166k★)
    │   ├── L2-musubi/              ← MUSUBI EARS + traceability
    │   ├── L2-openspec-brownfield/ ← OpenSpec delta-specs
    │   ├── L2-csdd-security/       ← CSDD overlay (SEC-001..SEC-007)
    │   └── L3-watch/               ← informational; "watch, don't deploy"
    └── stack/
        ├── dotnet/                 ← .NET 8/9 conventions + permissions
        ├── angular/                ← Angular 18+ + Playwright + permissions
        ├── unity/                  ← Unity 6 / 2023 LTS + Play Mode testing
        └── python/                 ← Python 3.12+ + uv + ruff + pytest
```

## Out of scope (v1)

- **BMAD-METHOD** — heavy ceremony; deferred to a possible v2 preset
- **Subagent orchestration platforms** (claude-flow, ccswarm) — capped by deliberate "limit multi-agent" constraint
- **L3 productive scaffolding** — Tessl closed beta + CSDD academic-only; L3 surfaces as `L3-watch` info overlay only
- **Frontend frameworks other than Angular** — React/Vue/Svelte out for v1
- **Mobile / IaC / ML notebooks** — out for v1
- **EY-specific overlay** — separate `mck-scaffold-ey` repo, loaded via `--overlay=ey`; not in this repo

## Roadmap

- **v1.0** — dogfood pass complete (see [TEST_PLAN.md](./TEST_PLAN.md)), known issues triaged, marketplace listing live
- **v1.1** — Phase 7.1: real hook content per stack/methodology (currently empty arrays); version pinning for upstream installs
- **v2** — BMAD preset, additional frontend frameworks if demand emerges, optional automated CI tests for the scaffolder itself

## License

MIT. See [LICENSE](./LICENSE) (TBD — to be added in v1.0 marketplace prep).

## Contributing

Personal project at v0.9.0. Not yet public. Once v1.0 ships and the marketplace listing is live, contribution guidelines will land here.

In the meantime, if you're reading this and want to suggest a methodology or stack adapter: open an issue describing the project context that *isn't* served by the current routing, with a concrete example. New presets land when there's a real project they'd serve, not speculatively.
