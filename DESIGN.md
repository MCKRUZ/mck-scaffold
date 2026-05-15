# mck-scaffold — Design

> A Claude Code plugin that interviews you about a new (or existing) project, then scaffolds the right Spec-Driven Development methodology — Superpowers, Spec-Kit, MUSUBI, OpenSpec, or a lean baseline — by routing on Piskala's three-level rigor taxonomy and Wasowski's four decision contexts.

**Status:** Phases 1–12 shipped 2026-05-15 — v0.11.0. Auto-install enforcement + research-refresh pipeline both live. mck-scaffold is feature-complete with marketplace manifest live. v1.0 ship is gated on the user executing `TEST_PLAN.md` against real projects and capturing audit-log findings. Decisions in §3 are locked. Defaults for §10 have been applied (see that section). Sections marked OPEN are still under discussion; everything else is built or scheduled.

---

## 1. Mission

**Install the right methodology, not the most popular one.**

The agentic-coding ecosystem in 2026 has 15+ SDD frameworks. Adoption is bimodal — Superpowers (~166k stars) and Spec-Kit (~90k) dominate popularity; MUSUBI (28 stars) and CSDD (academic prototype) dominate formal rigor. Choosing the wrong end of this spectrum is the most common dysfunction in real-world deployments. mck-scaffold makes the routing decision once, in an interview, and produces a coherent `.claude/` + `CLAUDE.md` + `AGENTS.md` + decision-doc trail for the chosen path.

The interview is the product. The templates are commodity.

---

## 2. Conceptual frame

### 2.1 Piskala's three-level SDD rigor taxonomy

Source: Piskala 2026 (arXiv:2602.00180), popularised in Wasowski 2026 (Medium).

| Level | Contract analogy | Promise | Cost | Frameworks |
|---|---|---|---|---|
| **L1 — Spec-First** | Letter of intent | Flexibility | Low | Spec-Kit, Superpowers, BMAD, GSD, cc-sdd, Don Cheli SDD, Agent OS, Shotgun, OWASP Skill |
| **L2 — Spec-Anchored** | BIM with addendums | Auditability + traceability | Medium-high | MUSUBI, Intent (Augment Code), OpenSpec |
| **L3 — Spec-as-Source** | CNC model | Guarantee by construction | Aspirational | Tessl, CSDD |

Higher rigor is not better. Higher rigor is more expensive, and only pays off when the project genuinely needs the promise.

### 2.2 Wasowski's four decision contexts

| Context | Profile | Default path |
|---|---|---|
| **A** | Solo / small team, greenfield | Spec-Kit lean preset ± Superpowers HARD-GATE |
| **B** | 10+ team, regulated, compliance | MUSUBI large + CSDD security overlay |
| **C** | Brownfield / legacy codebase | OpenSpec + Shotgun CLI |
| **D** | Security-critical (fintech, healthcare, embedded) | CSDD + MUSUBI + OWASP Skill |

L3 is out-of-scope as a live recommendation in 2026 — surfaced only as a "watch / not yet" branch when interview answers suggest L3 might matter in 18+ months.

### 2.3 Hidden axes worth treating as first-class questions

- **TDD-mandatory vs advisory** — meta-choice. Mandatory routes to Superpowers HARD-GATE (L1) or MUSUBI Article III (L2). Advisory routes to Spec-Kit / BMAD / lean.
- **MCP vs CLI orientation** — solo defaults CLI (Ronacher position); team defaults MCP (Shopify gateway pattern). User-locked constraint.
- **Multi-agent appetite** — capped at "limited" per locked constraint. Subagent-orchestration platforms (claude-flow, ccswarm) are out of v1 scope.

---

## 3. Locked architectural decisions

| Decision | Choice | Locked? |
|---|---|---|
| Project path | `C:\Users\RP325UG\OneDrive - EY\Documents\GitHub\MCKRUZ\mck-scaffold\` | ✅ |
| Form factor | Claude Code plugin (marketplace-installable) | ✅ |
| Composition | Meta-installer — routes to upstream frameworks; we ship the thin routing/config layer | ✅ |
| Conceptual frame | Piskala L1/L2/L3 + Wasowski's 4 contexts | ✅ |
| L3 inclusion | Surfaced as "watch / not yet" branch only | ✅ |
| EY preset | Out of mck-scaffold; lives in separate `mck-scaffold-ey` overlay, loaded via `--overlay=ey` | ✅ |
| Multi-agent cap | Limited (2-3 subagents max, all explicit) | ✅ |
| MCP routing | Team → MCP; Solo → CLI (interview decides per-project) | ✅ |

---

## 4. Repo layout

> **Schema correction applied 2026-05-15.** Anthropic's `example-plugin` documents that the `commands/` directory is the *legacy* format; modern plugins use `skills/<name>/SKILL.md` for both auto-triggered skills AND user-invoked slash commands (the difference is just frontmatter: add `argument-hint` and `allowed-tools` → it becomes a slash command). Layout updated accordingly. The plugin manifest also lives at `.claude-plugin/plugin.json` (in a subdirectory), not `.claude-plugin.json` at root.

```
C:\Users\RP325UG\OneDrive - EY\Documents\GitHub\MCKRUZ\
└── mck-scaffold\
    ├── .claude-plugin\
    │   └── plugin.json                    ← plugin manifest
    ├── README.md
    ├── DESIGN.md                          ← this file
    ├── CHANGELOG.md
    ├── skills\
    │   ├── init\
    │   │   └── SKILL.md                   ← user-invoked /mck-scaffold:init
    │   └── methodology-picker\
    │       └── SKILL.md                   ← auto-triggered conversational guidance
    ├── interview\
    │   ├── questions.md                   ← question tree (Q1..Q8) in human-readable form
    │   └── routing.md                     ← answer-combinations → preset mapping
    ├── templates\
    │   ├── shared\
    │   │   ├── CLAUDE.md.tmpl
    │   │   ├── AGENTS.md.tmpl
    │   │   ├── settings.json.tmpl
    │   │   └── docs\decisions\
    │   │       ├── INTERVIEW.md.tmpl
    │   │       └── METHODOLOGY.md.tmpl
    │   ├── methodologies\
    │   │   ├── L1-lean\
    │   │   ├── L1-spec-kit\
    │   │   ├── L1-superpowers\
    │   │   ├── L2-musubi\
    │   │   ├── L2-openspec-brownfield\
    │   │   ├── L2-csdd-security\          ← overlay, composes on top of L2-musubi
    │   │   └── L3-watch\                  ← informational only, never scaffolds code
    │   └── stack\
    │       ├── dotnet\
    │       ├── angular\
    │       ├── unity\
    │       ├── python\
    │       └── mixed\
    └── docs\
        ├── piskala-taxonomy.md            ← reference
        ├── wasowski-contexts.md           ← reference
        └── methodology-decision.md         ← how interview maps answers to output
```

---

## 5. The interview

### 5.1 Question tree

The interview asks one question at a time. Earlier answers prune later questions (e.g., brownfield routes straight to Context C, skipping questions that don't apply).

**Q1. Audit / compliance posture for this project?**
- (a) Never asked / no audit risk → **L1 candidate**
- (b) Sometimes audited / internal compliance review → **L2 candidate**
- (c) Always audited / external regulator → **L2 + CSDD security overlay candidate**
- (d) Security-critical (fintech, healthcare, automotive embedded) → **L2 + CSDD overlay + L3-watch flag**

**Q2. Greenfield or brownfield?**
- (a) Greenfield (new repo) → continue to Q3
- (b) Brownfield (existing codebase, adding to it) → **route to Context C: OpenSpec + Shotgun CLI**, skip Q3-Q4

**Q3. Team size?**
- (a) Solo / 2-3 people → **L1 lean defaults**
- (b) Small team (4-9) → **L1 with shared CLAUDE.md**
- (c) Team (10+) → **L2 candidate** (if Q1 ≥ b)

**Q4. TDD discipline?**
- (a) Mandatory, blocking → **Superpowers HARD-GATE** (L1) or **MUSUBI Article III** (L2)
- (b) Encouraged but not blocking → **Spec-Kit** (L1)
- (c) Off / will decide per-feature → **lean preset**

**Q5. Stack?** (multi-select)
- .NET / C#
- Angular / TypeScript
- Unity / game dev
- Python
- Mixed / multi-stack
- Other (free text)

**Q6. MCP gateway available?** (auto-defaults from Q3: team → yes, solo → no — user can override)
- (a) Yes → MCP path, allowlist template
- (b) No → CLI path, plain `gh` / curl / grep

**Q7. Multi-agent appetite?** (capped per locked constraint)
- (a) None — single Claude session per task
- (b) Limited — 2-3 subagents max, all explicit

**Q8. L3 monitoring branch?** (only shown when Q1 = d)
- (a) Yes — include L3-watch document so I know what to monitor
- (b) No — skip

### 5.2 Routing table

```
Q1   Q2     Q3      Q4    →  Preset
─────────────────────────────────────────────────────
a    green  solo    mand  →  L1-superpowers + stack
a    green  solo    enc   →  L1-spec-kit + stack
a    green  solo    off   →  L1-lean + stack
a    green  small   any   →  L1-spec-kit + stack
b    green  any     mand  →  L2-musubi (lite) + stack
b    green  team10  any   →  L2-musubi + stack
c    green  any     any   →  L2-musubi + L2-csdd-security overlay + stack
d    green  any     any   →  L2-musubi + L2-csdd-security overlay + L3-watch + stack
any  brown  any     any   →  L2-openspec-brownfield + stack
                                 (Shotgun CLI install triggered)
```

### 5.3 Re-run / idempotency rules

- Re-running `/mck-scaffold:init` on an existing project: detect prior `docs/decisions/INTERVIEW.md`, offer (a) reconfigure, (b) show diff vs current state, (c) abort.
- Files written but unchanged are skipped silently.
- Files changed locally are NEVER overwritten without explicit confirm — show the diff and ask.

---

## 6. Example outputs per context

### 6.1 Context A — solo greenfield, .NET API, TDD-mandatory

**Interview answers**: Q1=a, Q2=greenfield, Q3=solo, Q4=mandatory, Q5=.NET, Q6=CLI (solo), Q7=limited

**Resolved preset**: `L1-superpowers` + `stack/dotnet`

**Files written to target project:**
```
project\
├── CLAUDE.md                              ← lean, ~80 lines: stack rules + Superpowers reminders
├── AGENTS.md                              ← cross-tool baseline pointing at CLAUDE.md
├── .claude\
│   ├── settings.json                      ← hooks: pre-commit dotnet format, attribution off
│   └── rules\
│       ├── dotnet-conventions.md          ← from user's existing rules
│       └── tdd-discipline.md              ← TDD reminder (Superpowers enforces it)
└── docs\decisions\
    ├── INTERVIEW.md                       ← Q&A transcript + rationale
    └── METHODOLOGY.md                      ← why Superpowers + .NET adapter, alternatives considered
```

**Commands triggered:**
```
/plugin install superpowers@claude-plugins-official
```

### 6.2 Context B — enterprise compliance, Angular, team of 15

**Interview answers**: Q1=c (always audited), Q2=greenfield, Q3=team10+, Q4=mandatory, Q5=Angular, Q6=MCP, Q7=limited

**Resolved preset**: `L2-musubi` + `L2-csdd-security` overlay + `stack/angular`

**Files written:**
```
project\
├── CLAUDE.md                              ← MUSUBI invocation + Angular conventions
├── AGENTS.md
├── .claude\
│   ├── settings.json                      ← MCP allowlist, audit log hook, pre-commit
│   └── skills\                            ← MUSUBI skills land here via npx musubi-sdd init
├── docs\
│   ├── constitution.md                    ← 9 articles, CSDD security clauses appended
│   ├── specs\                             ← EARS-formatted requirement templates
│   └── decisions\
│       ├── INTERVIEW.md
│       ├── METHODOLOGY.md
│       └── audit\                         ← audit trail dir, populated by hooks
```

**Commands triggered:**
```
npx musubi-sdd@latest init
# CSDD overlay: append SEC-NNN clauses to docs/constitution.md from template
```

### 6.3 Context C — brownfield Unity hobby project

**Interview answers**: Q1=a, Q2=brownfield, Q5=Unity, Q6=CLI, Q7=none

**Resolved preset**: `L2-openspec-brownfield` + `stack/unity`

**Files written:**
```
project\
├── CLAUDE.md                              ← Unity conventions + OpenSpec delta-spec reminders
├── AGENTS.md
├── .claude\
│   ├── settings.json
│   └── rules\
│       └── unity-conventions.md           ← from user's existing rules
├── openspec\                              ← landed by openspec init
└── docs\decisions\
    └── INTERVIEW.md, METHODOLOGY.md
```

**Commands triggered:**
```
uvx shotgun plan "<short description user provides>"
openspec init
```

### 6.4 Context D — security-critical fintech (hypothetical, since you're EY)

**Interview answers**: Q1=d, Q2=greenfield, Q3=team10+, Q4=mandatory, Q5=mixed, Q6=MCP, Q7=limited, Q8=yes (show L3 watch)

**Resolved preset**: `L2-musubi` + `L2-csdd-security` overlay + `L3-watch` + `stack/mixed`

**Files written:**
```
project\
├── CLAUDE.md
├── AGENTS.md
├── .claude\settings.json                  ← MCP allowlist (financial APIs), audit hook, OWASP scan hook
├── docs\
│   ├── constitution.md                    ← CSDD security clauses, CWE references, MITRE Top 25
│   ├── specs\                             ← EARS templates with security patterns prominent
│   ├── decisions\
│   │   ├── INTERVIEW.md
│   │   ├── METHODOLOGY.md
│   │   └── audit\
│   └── horizon\
│       └── L3-WATCH.md                    ← Tessl/CSDD status, what to monitor, recommended
│                                            re-evaluation date (2027-Q1)
```

**Commands triggered:**
```
npx musubi-sdd@latest init
# CSDD overlay applied; OWASP Skill content appended to .claude/skills/
```

---

## 7. What each preset contributes

Each preset is a small folder of fragments that gets composed with `templates/shared/` and the chosen stack adapter. Presets don't write whole files — they contribute named fragments that the scaffolder merges.

| Preset | Adds to CLAUDE.md | Adds to settings.json | Installs | Adds doc artifacts |
|---|---|---|---|---|
| L1-lean | Plan Mode reminder, lean rule | — | nothing | minimal METHODOLOGY.md |
| L1-spec-kit | Constitution reference, 5-cmd workflow | — | `specify init --ai claude` | constitution.md, spec/plan/tasks templates |
| L1-superpowers | "use brainstorming skill first" reminder | — | `/plugin install superpowers@...` | docs/superpowers/specs/ dir |
| L2-musubi | MUSUBI orchestration cues, EARS reminder | audit log hook, pre-commit traceability check | `npx musubi-sdd@latest init` | constitution.md (9 articles), spec/design/tasks dirs |
| L2-openspec-brownfield | Delta-spec workflow cues | — | `uvx shotgun`, `openspec init` | openspec/ dir |
| L2-csdd-security (overlay) | Security-first reminder | OWASP scan hook on commit | — | CSDD clauses appended to constitution.md |
| L3-watch (overlay) | — | — | — | docs/horizon/L3-WATCH.md |

---

## 8. Stack adapters

Each stack adapter adds a `rules/<stack>-conventions.md` file plus a `settings.json` fragment with stack-appropriate hooks (e.g., .NET → `dotnet format` on pre-commit). Sourced from the user's existing `~/.claude/rules/*.md` library where possible.

| Stack | Hooks | Rules file content sourced from |
|---|---|---|
| .NET / C# | `dotnet format`, `dotnet build`, `dotnet test` on appropriate events | `coding-style.md`, `testing.md`, `security.md` |
| Angular / TypeScript | `ng lint`, `ng test --watch=false`, `npm audit` | `coding-style.md`, `testing.md` |
| Unity | Asset validation, Play Mode test runner reminder | `game-dev.md` |
| Python | `ruff check`, `pytest` | `coding-style.md`, `testing.md` |
| Mixed | Composed from above | All applicable |

---

## 9. Out of scope (v1)

- **BMAD-METHOD** — deferred. Heavy ceremony, smaller real-demand than its star count suggests. Worth a v2 preset if you actually use it.
- **Subagent orchestration platforms** (claude-flow, ccswarm, claude-swarm) — capped by your locked "limit multi-agent" constraint.
- **L3 productive scaffolding** — Tessl is closed beta, CSDD is a paper. We surface L3 as info only.
- **EY overlay** — separate repo `mck-scaffold-ey/`, loaded via `--overlay=ey`. Not built here.
- **Monorepo / multi-package special handling** — out of v1. Hierarchical CLAUDE.md is supported manually.
- **Non-Claude-Code targets** — AGENTS.md is written for cross-tool portability but we don't actively test against Codex/Cursor/Gemini in v1.

---

## 10. Pre-Phase-1 questions — defaults applied 2026-05-15

The user said "ok continue" after reviewing the design. Defaults below applied. All are reversible — flag any that need to change.

| # | Question | Default applied | Phase to revisit if wrong |
|---|---|---|---|
| 1 | Plugin slug for marketplace | `mck-scaffold` (matches folder name) | Phase 10 (marketplace prep) |
| 2 | Slash command name | `/mck-scaffold:init` (auto-derived from `plugin-name:skill-name`) | Phase 1 (now) |
| 3 | Plugin vs skill emphasis | All skills. `init` user-invoked; `methodology-picker` model-invoked with auto-trigger phrases. Commands directory dropped per Anthropic example-plugin guidance. | Phase 1 (now) |
| 4 | Version pinning for upstream installs | Pin to known-good in templates; quarterly upstream re-eval cadence | Phase 7 (meta-installer trigger logic) |
| 5 | Conflict handling when target already has framework | Warn and ask (option b); default abort | Phase 7 |
| 6 | First-cycle dogfood target | Deferred — decide when Phase 9 starts based on actual project at hand | Phase 9 |

---

## 11. Acceptance criteria for v1

mck-scaffold is "done v1" when:

- [ ] User can run `/plugin marketplace add` against this repo and install mck-scaffold
- [ ] `/mck-scaffold:init` runs the full Q1–Q8 interview in a fresh project folder
- [ ] At least 4 contexts produce coherent scaffolds (A solo TDD, B enterprise audit, C brownfield, D security-critical)
- [ ] Each scaffold includes a populated `docs/decisions/METHODOLOGY.md` explaining what was chosen and why
- [ ] Re-running on an existing scaffolded project shows a diff, doesn't clobber local changes
- [ ] At least 3 stack adapters work (.NET, Angular, Unity)
- [ ] README has a 60-second quickstart that an EY colleague could follow
- [ ] All upstream plugin installs are version-pinned with a documented re-eval cadence

---

## 12. Risk register

| Risk | Impact | Mitigation |
|---|---|---|
| Upstream framework breaking change (Superpowers, MUSUBI API change) | Scaffold succeeds, target project is broken | Version-pin; subscribe to upstream releases; quarterly re-eval |
| User runs interview, doesn't like recommendation, no easy escape | Abandonment | "I want something else" branch on every recommendation screen, with full list of alternatives |
| Interview feels like an interrogation (8 questions is a lot) | Friction, low adoption | Smart pruning (brownfield skips Q3-Q4), defaults shown so user can hit accept-all |
| Methodology choice ages poorly (e.g., Superpowers fragments) | Stale recommendations | `docs/decisions/METHODOLOGY.md` records the decision date so user can re-eval |
| MCP gateway assumption wrong for team context | Misconfigured settings.json | Always ask Q6 even when defaulted; show the default, let user override |
| Plugin installation in restricted enterprise environment fails | Hard stop | Document offline alternative (manually run the meta-installer commands); future v2 = self-contained fallback |
