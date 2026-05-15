# Changelog

All notable changes to mck-scaffold will be recorded here.

## [0.10.1] — 2026-05-15 — GitHub repo wiring

### Added
- `mck-scaffold/.claude-plugin/marketplace.json` — single-plugin marketplace manifest co-located with `plugin.json` (the context-mode pattern). Makes the GitHub repo directly install-able via `/plugin marketplace add github:MCKRUZ/mck-scaffold` without needing a separate marketplace repo.

### Changed
- `plugin.json` — `homepage` / `repository` updated from `MCKRUZ/MCKRUZ` placeholder to `https://github.com/MCKRUZ/mck-scaffold` (the real repo)
- `README.md` quickstart — GitHub install is now the canonical path; local-file install kept as "for development on the plugin itself"
- Template URL references updated across `CLAUDE.md.tmpl`, `AGENTS.md.tmpl`, `METHODOLOGY.md.tmpl`, `L3-WATCH.md`

### Design notes
- **Two install paths coexist:** GitHub (`github:MCKRUZ/mck-scaffold`) for users, local file path (`file:///…/MCKRUZ/`) for development. The parent `MCKRUZ/.claude-plugin/marketplace.json` is now redundant for users but stays put for local-only multi-plugin marketplace experimentation.
- **No automated push.** Git init + first commit + push is a "visible to others" action that warrants explicit user confirmation, not an autonomous step. Suggested commands documented; user runs them.

## [0.10.0] — 2026-05-15 — Phase 10: README polish &amp; marketplace prep

### Added
- `MCKRUZ/.claude-plugin/marketplace.json` — the marketplace manifest at the parent folder. Schema verified against `~/.claude/plugins/marketplaces/context-mode/.claude-plugin/marketplace.json` (single-plugin local-source pattern). Lists `mck-scaffold` with `source: "./mck-scaffold"`. Owner = Matthew Kruczek; description ready for the MCKRUZ folder to grow into a multi-plugin marketplace as additional plugins land.
- `mck-scaffold/LICENSE` — MIT, 2026.

### Changed — `mck-scaffold/README.md`
- Status banner updated to v0.9.0 with v1.0 gate on TEST_PLAN dogfood pass.
- Quickstart now uses the real marketplace install command (`/plugin marketplace add file:///…` then `/plugin install mck-scaffold`).
- Added compact interview-overview table (Q1–Q8 + routes-by).
- Added "Out of scope (v1)" section so users understand what they're NOT getting.
- Added "Roadmap" with v1.0 / v1.1 / v2 targets.
- Repo layout tree updated to match shipped state.
- Conceptual references expanded to include Anthropic's foundational articles.

### Design notes
- **Marketplace schema verified by file inspection**, not memory. Two patterns exist (single-plugin local-source vs multi-plugin URL-source); we chose local-source so MCKRUZ owns the plugin directly and the install path is one `marketplace add` away from working offline.
- **MCKRUZ marketplace is now multi-plugin-capable.** As you add more plugins to MCKRUZ/, append entries to the `plugins` array — no per-plugin marketplace boilerplate.
- **README intentionally short** (~120 lines). DESIGN.md is the comprehensive reference; README is the front door.
- **LICENSE is MIT.** If EY policy requires otherwise (Apache 2.0, BUSL, proprietary), swap before any public listing. The MCKRUZ folder is in OneDrive - EY so caution is warranted.

## [0.9.0] — 2026-05-15 — Phase 9: dogfood test plan

### Added
- `TEST_PLAN.md` — 11-section dogfood procedure: prerequisites → install → smoke test → 4 scenario tests (one per Wasowski Context A/B/C/D) → re-run/idempotency → conflict detection → audit-log template → acceptance criteria → known limitations.

### Design notes
- **Test plan is for the user to execute.** This conversation cannot install + run the plugin against real target projects; v1.0 acceptance is gated on the user running through the plan and capturing rough edges in `docs/audit-log.md`.
- **80% pass-criteria threshold** for v1.0 ship. Failing scenarios become Phase 7.1 / 7.2 iteration targets. This is honest — agentic scaffolders have edge cases that emerge only against real projects.
- **Audit-log template format included** so findings are captured consistently rather than as ad-hoc notes. Format: Date / Scenario / Repro / Expected / Actual / Hypothesis / Workaround / Files Touched.
- **Known limitations explicitly enumerated** in §11 so they don't masquerade as test failures: slash-command installs as manual steps (by design), empty hook arrays (Phase 7.1), `@latest` upstream pinning (Phase 7.1), no automated CI tests (v2 consideration).

## [0.8.0] — 2026-05-15 — Phase 8: L3-watch overlay

### Added — `templates/methodologies/L3-watch/`
- `README.md` — overlay scope, why "watch not deploy" in 2026 is principled, when composed (`Q1 = d` + `Q8 = yes` only)
- `horizon/L3-WATCH.md` — substitutable template; lands in target project as `docs/horizon/L3-WATCH.md`. Includes: Piskala taxonomy recap, current-setup recap, status table of Tessl/CSDD/ecosystem maturity, what-to-monitor list (Tessl beta exit, CSDD empirical replication, MCP/AGENTS.md standardization, Kiro/BMAD adjacent signals), migration considerations when L3 is ready (6 concrete cost areas), explicit "what this doc does NOT do" boundary.

### Design notes
- L3-watch is a **pure information overlay** — no CLAUDE.append.md, no settings.merge.json, no upstream install. Purely documentary. The scaffold writes one file (`docs/horizon/L3-WATCH.md`) and that's the entire overlay surface.
- Re-eval date (`{{re_eval_date}}`) is the operative hook — without it, the doc rots. With it, the user has a calendar prompt to come back and re-check.
- Empirical claims (Tessl 10k registry, CSDD 73% reduction) are flagged as single-source / vendor figures. Epistemic discipline carries through to L3 content.
- The "what to monitor" section is specifically actionable — "verify by sampling 10 random libraries from your stack" beats "watch for ecosystem maturity," and "three real production cases is the minimum signal" beats "is it production-ready?"

## [0.7.0] — 2026-05-15 — Phase 7: meta-installer trigger logic

### Changed — `skills/init/SKILL.md`
Major rewrite. Phase 2 stopped at the decision summary; Phase 7 carries the pipeline through actual scaffolding and upstream installs.

- **Pipeline structure now 9 steps:** Steps 1–6 (interview, unchanged) → confirmation point → Steps 7–9 (scaffold + install + summary)
- **Step 7 (execute scaffolding)** broken into 7a–7h:
  - 7a: read all template fragments (shared + methodology + overlays + each stack)
  - 7b: compose CLAUDE.md (shared base → methodology `CLAUDE.append.md` → overlay appends → stack appends in Q5 selection order). Verifies no `{{...}}` placeholders remain.
  - 7c: compose AGENTS.md
  - 7d: compose `.claude/settings.json` via deep-merge with append-and-dedup rules for permissions arrays
  - 7e: write `docs/decisions/INTERVIEW.md`
  - 7f: write `docs/decisions/METHODOLOGY.md` (re-substituted in Step 9 to fill installed-plugins list)
  - 7g: L3-watch overlay → `docs/horizon/L3-WATCH.md` (template ships in Phase 8)
  - 7h: write contract — existing files NEVER silently overwritten; diff + AskUserQuestion (overwrite/skip/abort), default skip
- **Step 8 (trigger upstream installs)** broken into 8a–8d:
  - 8a: bash-runnable commands (`specify init`, `npx musubi-sdd@latest init`, `uvx shotgun plan`, `openspec init`) with per-install conflict pre-check and post-install marker verification
  - 8b: slash-command installs (`/plugin install superpowers@...`) surfaced as manual user steps — a skill body cannot programmatically invoke slash commands; honest separation
  - 8c: version pinning policy (deferred to Phase 7.1 iteration; Phase 7 ships with `@latest` per upstream marketplace)
  - 8d: CSDD `constitution.append.md` appended AFTER MUSUBI's `docs/constitution.md` is created — explicit ordering constraint
- **Step 9 (final summary)** — files-written list + installed-plugins list (including `(manual)` items) + manual steps still required + re-eval cadence + preset-specific "next" pointer
- **Idempotency / re-run subroutine** when `docs/decisions/INTERVIEW.md` exists: four options (reconfigure from scratch / refresh templates with same answers / show diff / abort). Default abort. Re-runs are destructive; explicit opt-in required.
- **`--dry-run` argument** now functional — forces dry-run path at Step 6, skips Steps 7–8 entirely
- **Full variable dictionary documented** in Step 7 — every `{{var}}` placeholder from the template schema accounted for

### Design notes
- **Slash-command installer separation is principled.** A skill body cannot invoke `/plugin install` programmatically. Surface explicitly as a manual step with `(manual)` marker rather than pretending we can run them. Trust-but-verify model.
- **Conflict handling default = skip**, not abort. Skipping the install for a framework the user already has lets the rest of the scaffold proceed. Abort is reserved for explicit user choice.
- **Partial install tolerance.** If one upstream install fails, log it and continue with the next. Don't abort the whole scaffold on one upstream failure. The final summary surfaces what worked and what didn't.
- **Variable substitution verification.** Step 7b explicitly verifies no `{{...}}` placeholders remain in the final composed CLAUDE.md before writing. Substitution gaps abort the write — better to fail loudly than ship a broken CLAUDE.md.
- **Order constraint locked: files first, installs second, CSDD-append last.** CSDD constitution clauses can only be appended after MUSUBI creates the base constitution. Step 8d explicitly waits for the marker file.

### Outstanding (Phase 7.1 iteration when problems materialize)
- Hard version pinning for upstream installs (currently `@latest`)
- MCP overlay fragment (currently a no-op when `mcp_orientation = yes`)
- Diff-display format polish (truncation thresholds, hunk size)
- Bash install timeout calibration per upstream tool

## [0.6.0] — 2026-05-15 — Phase 6: stack adapters

### Added — `templates/stack/dotnet/`
- `README.md` — when-composed, assumptions (.NET 8/9, xUnit, FluentValidation + MediatR, EF Core)
- `CLAUDE.append.md` — immutability (IReadOnly\*, init-only, records), Clean Architecture file org, naming, Result\<T\>, FluentValidation auto-discovery, keyed DI, project defaults (ImplicitUsings + Nullable), xUnit + WebApplicationFactory testing, security (User Secrets / Key Vault, EF Core parameterization, JWT validation, CORS allowlist), vulnerability checks
- `settings.merge.json` — `permissions.allow`: `dotnet:*`, `nuget:*`

### Added — `templates/stack/angular/`
- `README.md` — when-composed, assumptions (Angular 18+, NgRx/signals, Jasmine + Karma, Playwright)
- `CLAUDE.append.md` — spread/NgRx immutability, kebab-case files + observable `$` suffix, catchError + logger service, Reactive Forms, HttpTestingController with `afterEach(verify)`, Playwright with `data-testid`, XSS posture + DomSanitizer justification rule, dependency hygiene
- `settings.merge.json` — `permissions.allow`: `ng:*`, `npm:*`, `npx playwright:*`, `npx ng:*`

### Added — `templates/stack/unity/`
- `README.md` — when-composed, assumptions (Unity 6 / 2023 LTS, URP/HDRP, NVIDIA RTX Kit local path)
- `CLAUDE.append.md` — Assembly Definitions, ScriptableObjects, prefab-first, `[SerializeField] private`, MonoBehaviour vs pure C# split, object pooling, events over direct refs, 16.67 / 33.33 ms frame budget, shader development notes (ShaderLab + HLSL, Shader Graph for artists), NVIDIA integration (DLSS via UPM, Nsight Systems, RTX Kit), Play Mode + performance testing, build hygiene
- `settings.merge.json` — minimal (Unity has poor CLI integration)

### Added — `templates/stack/python/`
- `README.md` — when-composed, assumptions (Python 3.12+, `uv`, `ruff`, `pytest`)
- `CLAUDE.append.md` — naming + type hints + immutable-by-default, `src/` layout, pytest + fixtures + parametrize, ruff + uv tooling, custom exception types, logging not print, asyncio vs threading rule, dependency hygiene
- `settings.merge.json` — `permissions.allow`: `python:*`, `python3:*`, `pytest:*`, `ruff:*`, `uv:*`, `uvx:*`, `pipx:*`

### Updated — `templates/stack/README.md`
- Removed Phase 6 placeholder; documents the 4 shipped adapters + the multi-stack composition pattern (no `mixed/` subfolder; scaffolder concatenates selected adapters)
- Explicit "out of v1 scope" list (React/Vue/Svelte, mobile, IaC, ML notebooks)
- Composition order recap for both `CLAUDE.md` and `settings.json`

### Design notes
- All stack content lifted verbatim or near-verbatim from user's `~/.claude/rules/` — single source of truth for personal conventions; mck-scaffold packages them for distribution to scaffolded projects rather than re-deriving.
- No `mixed/` subfolder created. Multi-stack is handled by composition in the scaffolder, not by a separate adapter. Documented in `templates/stack/README.md`.
- Stack `settings.merge.json` files focus on `permissions.allow` entries (stable schema). Hook arrays left empty until Phase 7 verifies exact Claude Code hook syntax against installed plugin examples.
- Composition order locked: shared → methodology → overlays → stacks (in selection order). Later layers append; conflicts where two stacks configure the same hook → later-selected wins.

## [0.5.0] — 2026-05-15 — Phase 5: L2 presets

### Added — `templates/methodologies/L2-musubi/`
- `README.md` — when-to-pick + EARS quick reference + caveats (28 stars is niche-not-bad; EARS learning curve; bus-factor risk)
- `CLAUDE.append.md` — full MUSUBI workflow (@requirements-analyst → @system-architect → @test-engineer → implement → @traceability-auditor → @constitution-enforcer); EARS examples; traceability matrix format; brownfield delta (@change-impact-analyzer); when-NOT-to-use-full-ceremony list
- `settings.merge.json` — hook arrays scaffolded for pre-commit traceability check + audit log

### Added — `templates/methodologies/L2-openspec-brownfield/`
- `README.md` — brownfield-only routing rationale + caveats (Shotgun indexing time, lighter-than-MUSUBI, one-maintainer bus-factor, doesn't compose with CSDD overlay)
- `CLAUDE.append.md` — Shotgun-first workflow, delta-spec format (ADDED / MODIFIED / REMOVED) with example, brownfield discipline rules, when-NOT-to-use
- `settings.merge.json` — minimal hook stubs

### Added — `templates/methodologies/L2-csdd-security/` (overlay)
- `README.md` — overlay nature (composes on L2-musubi only), what-it-adds-to-MUSUBI table, honest caveats (73% number is single-study, you'll tune for a week, static analysis tool not provided, brownfield mismatch, re-eval cadence)
- `CLAUDE.append.md` — security-first reminder, SEC-001 through SEC-007 quick reference table, Compliance Traceability Matrix pointer, workflow integration
- `constitution.append.md` — full SEC-001 through SEC-007 clauses (CWE-89 SQL injection, CWE-79 XSS, CWE-798 hardcoded creds, CWE-502 deserialization, CWE-310 crypto, CWE-22 path traversal, CWE-352 CSRF) with Level/Rule/Enforcement/Violation format. Plus Variance ADR procedure for documented exceptions.
- `settings.merge.json` — hook arrays scaffolded for OWASP scan + secrets-scan pre-commit

### Design notes
- New file convention: `constitution.append.md` for overlays that add clauses to the primary preset's constitution. The scaffolder appends; preset's constitution is the base.
- CSDD overlay deliberately does NOT compose with L2-openspec-brownfield. Mixing CSDD's MUSUBI-shaped constitution with OpenSpec's delta-spec workflow produces incoherent artifacts. For brownfield + security-critical, the recommendation is "migrate to MUSUBI proper first" — surfaced to user in the overlay README.
- SEC clauses include a **Variance procedure** — ADR-tracked exceptions when a clause genuinely can't be satisfied. Silent variances are violations; ADR-tracked variances are auditable. Important for real-world adoption.
- The Marri 73% claim is flagged explicitly as single-study, not industry-standard, in the overlay README. Epistemic discipline matters when shipping security guidance.

## [0.4.0] — 2026-05-15 — Phase 4: L1 presets

### Added — `templates/methodologies/L1-lean/`
- `README.md` — when-to-pick + caveats (lean discipline only works if user holds the line)
- `CLAUDE.append.md` — "Plan Mode is enough"; escape valves to escalate to L1-superpowers or L1-spec-kit when the discipline slips
- `settings.merge.json` — empty hook stubs

### Added — `templates/methodologies/L1-spec-kit/`
- `README.md` — when-to-pick + caveats (~10x first-cycle overhead, stale PR queue, bad-fit-for-legacy)
- `CLAUDE.append.md` — 5-command pipeline (`/speckit.constitution` → specify → plan → tasks → implement); constitution-as-contract framing; Articles I/III/VI highlighted
- `settings.merge.json` — empty hook stubs (Spec-Kit populates `.specify/` directly)

### Added — `templates/methodologies/L1-superpowers/`
- `README.md` — when-to-pick + full caveats (HN "stripped back to 30%", overkill for throwaways, Claude-Code-only, no traceability, no brownfield, star count fast-moving)
- `CLAUDE.append.md` — workflow walkthrough (brainstorm → plan → execute), HARD-GATE rule verbatim, TDD iron law verbatim, direct invocation reference
- `settings.merge.json` — empty hook stubs (Superpowers ships its own bundle via plugin install)

### Design notes
- Each preset's `CLAUDE.append.md` is deliberately SHORT. The upstream framework IS the content; we don't duplicate it. We add the project-specific glue: when to invoke, escape valves, honest caveats.
- L1-superpowers' settings.merge.json is empty — Superpowers' plugin install does all the work. No need to redundantly configure.
- Caveats are first-class. Each README has a dedicated "Honest caveats" section that surfaces in the target project's `METHODOLOGY.md`. Users see the trade-offs at scaffold time, not after they've committed.

## [0.3.0] — 2026-05-15 — Phase 3: shared base templates

### Added
- `templates/shared/CLAUDE.md.tmpl` — lean base CLAUDE.md (~25 lines). Plan Mode reminder, methodology/stack content slots, AGENTS.md link, decision-doc pointers.
- `templates/shared/AGENTS.md.tmpl` — cross-tool agent config (Codex/Cursor/Gemini/Copilot). Points at CLAUDE.md as authoritative.
- `templates/shared/settings.json.tmpl` — minimum valid `.claude/settings.json`. Empty hook arrays, `attribution.commit: ""`, empty permissions. Presets and stacks append.
- `templates/shared/docs/decisions/INTERVIEW.md.tmpl` — permanent record of Q1–Q8 answers. Used by re-run detection.
- `templates/shared/docs/decisions/METHODOLOGY.md.tmpl` — rationale, alternatives, scaffolded files, caveats, 6-month re-eval cadence.
- `templates/shared/README.md` — full template variable schema documented (project context, methodology resolution, interview answers, decision-doc generation) plus composition order rules.

### Design notes
- `{{variable_name}}` placeholder convention chosen (Mustache-style, intuitive). All .tmpl files are valid markdown/JSON with placeholders intact — they parse and render correctly even pre-substitution.
- Composition order locked: **shared base → methodology preset → overlays → stack adapters**. Later layers override earlier where keys collide; arrays append; objects deep-merge.
- `settings.json.tmpl` deliberately metadata-free. No `_mck_scaffold` debug keys leaking into target projects. Scaffold metadata lives in `docs/decisions/METHODOLOGY.md` only.
- Re-eval cadence baked into METHODOLOGY.md template — 6 months default. The ecosystem is moving too fast for permanent decisions.

## [0.2.0] — 2026-05-15 — Phase 2: interview &amp; routing engine

### Added
- `skills/init/SKILL.md` upgraded from print-stub to functional interview executor:
  - Step 1: detects project context (`.git`, existing CLAUDE.md, prior SDD markers)
  - Step 2: loads `interview/questions.md` + `interview/routing.md` as source of truth
  - Step 3: drives Q1–Q8 via `AskUserQuestion`, one question at a time, with pruning rules applied
  - Step 4: deterministic routing — same answers always resolve to the same preset
  - Step 5: renders decision summary markdown block (interview answers + resolved methodology + rationale + scaffold preview + honest caveats)
  - Step 6: confirm / next-closest preset / tweak-an-answer iteration loop
  - Re-run on folder with prior `docs/decisions/INTERVIEW.md` → abort cleanly (reconfigure/diff lands in Phase 7)
- `skills/methodology-picker/SKILL.md` — dropped Phase 1 stub note; now reflects full Phase 2 capability

### Design notes
- Markdown as single source of truth for both conversational (`methodology-picker`) and deterministic (`init`) paths. No parallel JSON.
- Phase 2 still writes nothing to the target project. The decision summary is conversational output. Phase 3 adds the templates this summary consumes.
- `init/SKILL.md` includes explicit "Implementation notes for Claude" section forbidding Write/Edit/Bash in Phase 2 — Phase 7 unlocks the meta-installer.

## [0.1.0] — 2026-05-15 — Phase 1: skeleton

Initial scaffolding of the plugin itself.

### Added
- `.claude-plugin/plugin.json` — plugin manifest
- `README.md`, `DESIGN.md`, `CHANGELOG.md` — top-level docs
- `skills/init/SKILL.md` — user-invoked `/mck-scaffold:init` stub (prints planned behavior, writes nothing)
- `skills/methodology-picker/SKILL.md` — model-invoked skill that auto-triggers on project-setup phrases for conversational guidance
- `interview/questions.md` — Q1–Q8 question tree (stub, sourced from DESIGN.md §5.1)
- `interview/routing.md` — answer-combination → preset mapping (stub, sourced from DESIGN.md §5.2)
- Empty directory placeholders for `templates/shared/`, `templates/methodologies/`, `templates/stack/`, `docs/`

### Design decisions
- Modern `skills/` layout used throughout; legacy `commands/` directory dropped per Anthropic example-plugin guidance
- `project-setup` skill collapsed into `init` (redundant)
- Plugin author/email set to Matthew Kruczek; license MIT
- Phase 1 stub writes nothing — verifiable that plugin loads and slash command resolves before any real scaffolding logic ships
