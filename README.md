# mck-scaffold

> Interview-driven project scaffolder for Claude Code. Picks the right Spec-Driven Development methodology for your project — instead of installing the most popular one.

**Status:** v0.11.0 — auto-install enforcement + research-refresh pipeline live. v1.0 ships after [TEST_PLAN.md](./TEST_PLAN.md) dogfood pass on additional scenarios beyond the first one (logged in `docs/audit-log.md`).

## What it does

You run `/mck-scaffold:init` in a project folder. It:

1. **(Step 0)** Reads `research/last-run.json` and offers to refresh the framework catalog if it's stale (skip-default if < 30 days, neutral 30–90, strongly-recommended-default > 90).
2. **(Steps 1–6)** Walks you through an 8-question interview, applies Piskala's three-level rigor taxonomy + Wasowski's four decision contexts, routes deterministically to a methodology preset (Superpowers / Spec-Kit / MUSUBI / OpenSpec / lean), and shows a confirmable decision summary.
3. **(Step 7)** Composes your project's `CLAUDE.md`, `AGENTS.md`, `.claude/settings.json`, and `docs/decisions/{INTERVIEW,METHODOLOGY}.md` from shared base + methodology fragment + stack adapter, with full conflict resolution on existing files.
4. **(Step 8)** **Auto-runs** the upstream framework install (`specify init`, `npx musubi-sdd init`, `openspec init`, etc.) — not a manual step, the scaffolder does it.
5. **(Step 9)** Prints a final summary including any post-install advisories the upstream tool surfaced (e.g., Spec-Kit's `.claude/` credentials-gitignore note).

The interview is the product. The templates are commodity. The research pipeline keeps both fresh.

## Why

The agentic-coding ecosystem (mid-2026) has 15+ SDD frameworks. Adoption is bimodal — Superpowers (~166k stars) and Spec-Kit (~90k) dominate popularity; MUSUBI (28 stars) and CSDD (academic) dominate formal rigor. Choosing the wrong end of this spectrum is the most common dysfunction in real deployments.

mck-scaffold makes the routing decision once, with full visibility into the tradeoffs, and produces a coherent setup with a durable decision trail.

**And** the framework catalog itself drifts fast. The first dogfood run (on consultant-helper, 2026-05-15) immediately surfaced Spec-Kit drift — less than 30 days after the source article published. The research-refresh pipeline catches this kind of drift before routing decisions get made against stale data.

## Quickstart

```
# 1. Add the marketplace (one-time, host-wide)
/plugin marketplace add github:MCKRUZ/mck-scaffold

# 2. Install
/plugin install mck-scaffold

# 3. Use it
cd /path/to/new-or-existing-project
claude
> /mck-scaffold:init
```

For a *guidance-only* conversation (no file writes), describe what you want — the `methodology-picker` skill auto-triggers on phrases like "set up a new project," "pick a methodology," "what SDD framework should I use":

```
> I'm starting a new Angular API + .NET service for a regulated fintech client. What methodology should I use?
```

For a *dry run* of the scaffolder (interview only, no writes):

```
> /mck-scaffold:init --dry-run
```

To manually refresh the framework catalog (between init runs):

```
> /mck-scaffold:refresh-research
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

## The research-refresh pipeline (new in v0.11.0)

Before any interview question, Step 0 reads `research/last-run.json` and decides how to prompt about staleness. If you opt in, the `research-refresh` skill:

1. Launches 4 parallel research subagents (GitHub / Reddit / YouTube / web-blogs)
2. Each agent re-surveys its source for catalog drift (new frameworks, deprecated install commands, changed star counts, etc.)
3. Pipeline diffs new findings against the current `research/streams/*.md` baseline
4. Per-section confirm prompts let you apply changes selectively (no silent updates)
5. Confirmed changes update `routing.md` + methodology preset templates + the stream files themselves
6. `last-run.json` timestamp + `changelog.md` entry record the refresh

The research store lives **in the repo** (`research/`) — a refresh is essentially a contribution to the plugin, benefiting all users on their next pull.

## Conceptual references

- **Piskala 2026** ([arXiv:2602.00180](https://arxiv.org/abs/2602.00180)) — three-level SDD rigor taxonomy (L1 Spec-First / L2 Spec-Anchored / L3 Spec-as-Source)
- **Wasowski 2026** (Medium) — *Comparing 15 SDD Frameworks*, four decision contexts
- **Marri 2026** ([arXiv:2602.02584](https://arxiv.org/abs/2602.02584)) — Constitutional SDD (security overlay)
- **Anthropic** — *Building Effective Agents*, *Effective Context Engineering*, *Best Practices for Claude Code*

## Repo layout

```
mck-scaffold/
├── .claude-plugin/{plugin.json,marketplace.json}
├── README.md                       ← you are here
├── DESIGN.md                       ← full design and decisions
├── CHANGELOG.md                    ← phase-by-phase progress
├── TEST_PLAN.md                    ← dogfood procedure
├── LICENSE                         ← MIT
├── skills/
│   ├── init/SKILL.md               ← /mck-scaffold:init — interview + scaffold + install
│   ├── methodology-picker/SKILL.md ← conversational guidance (auto-trigger)
│   └── research-refresh/SKILL.md   ← /mck-scaffold:refresh-research — catalog refresh
├── interview/
│   ├── questions.md                ← Q1-Q8 tree
│   └── routing.md                  ← answer-combination → preset (with platform-aware install commands)
├── research/                       ← shared catalog state
│   ├── README.md                   ← what's here + how a refresh works
│   ├── last-run.json               ← timestamp + summary
│   ├── findings.md                 ← consolidated narrative
│   ├── changelog.md                ← deltas over time
│   └── streams/{github-landscape,reddit-signal,youtube-tutorials,web-blogs}.md
├── templates/
│   ├── shared/                     ← base CLAUDE.md / AGENTS.md / settings / decision docs + variable schema
│   ├── methodologies/
│   │   ├── L1-{lean,spec-kit,superpowers}/
│   │   ├── L2-{musubi,openspec-brownfield,csdd-security}/
│   │   └── L3-watch/               ← informational only
│   └── stack/{dotnet,angular,unity,python}/
└── docs/
    ├── README.md
    └── audit-log.md                ← real-world dogfood findings
```

## Out of scope (v1)

- **BMAD-METHOD** — heavy ceremony; deferred to a possible v2 preset
- **Subagent orchestration platforms** (claude-flow, ccswarm) — capped by deliberate "limit multi-agent" constraint
- **L3 productive scaffolding** — Tessl closed beta + CSDD academic-only; L3 surfaces as `L3-watch` info overlay only
- **Frontend frameworks other than Angular** — React/Vue/Svelte out for v1
- **Mobile / IaC / ML notebooks** — out for v1
- **EY-specific overlay** — separate `mck-scaffold-ey` repo, loaded via `--overlay=ey`; not in this repo

## Roadmap

- **v1.0** — broader TEST_PLAN dogfood (Wasowski contexts B/C/D), known issues triaged
- **v1.1 / Phase 7.1** — real hook content per stack/methodology (currently empty arrays); upstream-version pinning
- **v1.1 / Phase 12.1** — machine-parseable research-refresh diff format so `--auto-apply` can actually patch templates
- **v1.2 / Phase 13** — scheduled background research-refresh (monthly cadence default) via a hook
- **v2** — BMAD preset; additional frontend frameworks if demand emerges; optional automated CI tests for the scaffolder itself

## License

MIT. See [LICENSE](./LICENSE).

## Contributing

Personal project at v0.11.0. Not yet broadly public. The shared-repo research-refresh design means any user running a refresh is contributing to the catalog — open a PR with the refreshed `research/streams/` files + the updated `routing.md` and preset templates.
