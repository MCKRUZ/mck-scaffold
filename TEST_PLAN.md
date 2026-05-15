# mck-scaffold — Test Plan

> Dogfood procedure. Follow these steps in order. Report findings inline in `docs/audit-log.md` (created on first failure).

**Author requirement:** the test plan is written for *you* to execute. Each scenario has an explicit "pass" definition; deviations are captured in the audit log.

---

## 0 — Prerequisites

- Claude Code 2.0.13+ installed (`claude --version`)
- `git` on PATH
- Optional but useful for full scenario coverage: `node` (for `npx`), `python` + `uv` (for `uvx`), `.NET SDK` (for any .NET stack scenario)
- A scratch parent folder for test projects, e.g., `C:\scratch\mck-scaffold-tests\`

---

## 1 — Install the plugin

Run from any directory:

```
claude
/plugin marketplace add file:///C:/Users/RP325UG/OneDrive%20-%20EY/Documents/GitHub/MCKRUZ/
/plugin install mck-scaffold
```

**Pass criteria:**
- `/help` lists `/mck-scaffold:init` as an available slash command
- `methodology-picker` skill auto-triggers when you type something like "I want to set up a new project" (smoke test of the skill description)

If either fails, capture the error in `docs/audit-log.md` under "Install issues" before continuing.

---

## 2 — Smoke test (the interview only)

```
mkdir C:\scratch\mck-test-smoke
cd C:\scratch\mck-test-smoke
claude
/mck-scaffold:init --dry-run
```

Walk through Q1–Q8 with arbitrary answers. Use `--dry-run` to keep Phase 2 behavior — interview produces decision summary, no files written.

**Pass criteria:**
- Each question presented one at a time via `AskUserQuestion`
- Q5 (stack) is multi-select; Q1–Q4, Q6–Q8 are single-select
- Brownfield answer in Q2 skips Q3 and Q4 (pruning works)
- Q1=d shows Q8; Q1≠d skips Q8
- Final decision summary block renders with answers table + resolved methodology + rationale + scaffold preview + honest caveats
- No files written under `C:\scratch\mck-test-smoke\`

**Common failure modes to watch for:**
- Same answers produce different presets in two runs → routing not deterministic (Phase 7 acceptance violation)
- Decision summary references `{{...}}` placeholders → variable dictionary incomplete
- Pruning rules fire incorrectly → questions.md/routing.md interpretation bug

---

## 3 — Scenario A: solo greenfield, .NET, TDD-mandatory

Maps to Wasowski's **Context A**. Expected route: `L1-superpowers` + `dotnet` adapter.

```
mkdir C:\scratch\mck-test-A
cd C:\scratch\mck-test-A
claude
/mck-scaffold:init
```

**Answers:**
| Question | Answer |
|---|---|
| Q1 | (a) None — no audit risk |
| Q2 | Greenfield |
| Q3 | Solo |
| Q4 | Mandatory |
| Q5 | .NET / C# |
| Q6 | No (CLI) |
| Q7 | None |

**Confirm** at Step 6. Phase 7 executes Steps 7-9.

**Pass criteria:**
- Resolved preset: `L1-superpowers`
- Files written to `C:\scratch\mck-test-A\`:
  - `CLAUDE.md` (contains both Superpowers methodology section AND .NET stack section; no `{{...}}` placeholders)
  - `AGENTS.md`
  - `.claude\settings.json` (valid JSON; `permissions.allow` includes `Bash(dotnet:*)`, `Bash(nuget:*)`)
  - `docs\decisions\INTERVIEW.md` (table reflects answers above)
  - `docs\decisions\METHODOLOGY.md` (rationale paragraphs name `L1-superpowers` and `dotnet`; alternatives list mentions `L1-spec-kit` and `L1-lean`; caveats include the HN "stripped back to 30%" note)
- Manual-steps callout in final summary instructs running `/plugin install superpowers@claude-plugins-official` + `/plugin reload`
- Running `/plugin install superpowers@claude-plugins-official` succeeds (this is a real Anthropic-marketplace install)

**Audit log if any pass criterion fails.**

---

## 4 — Scenario B: enterprise compliance, Angular, team of 15

Maps to Wasowski's **Context B**. Expected route: `L2-musubi` + `L2-csdd-security` overlay + `angular` adapter.

```
mkdir C:\scratch\mck-test-B
cd C:\scratch\mck-test-B
claude
/mck-scaffold:init
```

**Answers:**
| Question | Answer |
|---|---|
| Q1 | (c) Always audited / external regulator |
| Q2 | Greenfield |
| Q3 | Team (10+) |
| Q4 | Mandatory |
| Q5 | Angular / TypeScript |
| Q6 | Yes (MCP) |
| Q7 | Limited |

**Confirm** at Step 6.

**Pass criteria:**
- Resolved preset: `L2-musubi` + `L2-csdd-security` overlay
- Files written including:
  - `CLAUDE.md` with MUSUBI methodology section + CSDD security overlay section + Angular stack section
  - `docs\specs\` directory created (by `npx musubi-sdd init` — verify the upstream install ran)
  - `docs\constitution.md` created by MUSUBI, then **appended** with CSDD SEC-001 through SEC-007 clauses
  - `.claude\settings.json` permissions include `Bash(ng:*)`, `Bash(npm:*)`, `Bash(npx playwright:*)`
- Step 8d (CSDD post-install append) ran AFTER MUSUBI install completed
- Final summary lists `npx musubi-sdd@latest init` as installed (bash-runnable) — no `/plugin install` slash command for this scenario

**Watch for:** Step 8d skipping because `docs\constitution.md` didn't appear at the expected path. If MUSUBI puts the constitution elsewhere, the append logic needs adjustment.

---

## 5 — Scenario C: brownfield Unity hobby project

Maps to Wasowski's **Context C**. Expected route: `L2-openspec-brownfield` + `unity` adapter.

**Setup:** create a fake brownfield context (don't run against a real Unity project on the first pass — verify the routing first).

```
mkdir C:\scratch\mck-test-C
cd C:\scratch\mck-test-C
git init
# create a fake Assets/ folder so it looks vaguely Unity-shaped
mkdir Assets Packages ProjectSettings
echo "" > Assets\.keep
claude
/mck-scaffold:init
```

**Answers:**
| Question | Answer |
|---|---|
| Q1 | (a) None |
| Q2 | Brownfield |
| Q5 | Unity / game dev |
| Q6 | No |
| Q7 | None |

**Pruning check:** Q3 and Q4 should be **skipped** (brownfield rule).

**Pass criteria:**
- Q3, Q4 not asked
- Resolved preset: `L2-openspec-brownfield`
- Final summary prompts for Shotgun's `"<short description>"` argument before running
- `uvx shotgun plan` runs (may take 5–30 minutes on a real codebase; cancel if it's clearly looping — record the failure mode)
- `openspec init` runs and creates `openspec/`
- Unity stack adapter content composed into `CLAUDE.md`

**Optional follow-up:** repeat against a real Unity hobby project for end-to-end realism. Capture rough edges.

---

## 6 — Scenario D: security-critical fintech (hypothetical)

Maps to Wasowski's **Context D**. Expected route: `L2-musubi` + `L2-csdd-security` + `L3-watch` overlays.

```
mkdir C:\scratch\mck-test-D
cd C:\scratch\mck-test-D
claude
/mck-scaffold:init
```

**Answers:**
| Question | Answer |
|---|---|
| Q1 | (d) Security-critical |
| Q2 | Greenfield |
| Q3 | Team (10+) |
| Q4 | Mandatory |
| Q5 | Mixed / multi-stack |
| Q6 | Yes |
| Q7 | Limited |
| Q8 | Yes (show me L3 watch) |

**Pass criteria:**
- Q8 was asked (Q1=d trigger)
- Resolved preset: `L2-musubi` + `L2-csdd-security` + `L3-watch`
- `docs\horizon\L3-WATCH.md` written with substituted `{{scaffold_date}}`, `{{re_eval_date}}`, `{{methodology_preset}}`
- L3-watch document includes the "what to monitor" list and the migration considerations
- No upstream install for L3-watch (it's purely informational)

---

## 7 — Re-run / idempotency tests

After Scenario A completes, re-run in the **same folder**:

```
cd C:\scratch\mck-test-A
claude
/mck-scaffold:init
```

**Pass criteria:**
- Step 1 detects `docs\decisions\INTERVIEW.md` from prior scaffold
- AskUserQuestion offers four options: reconfigure / refresh templates / show diff / abort
- Default selection is **abort**
- Choosing "show diff" prints unified diffs without writing anything
- Choosing "refresh templates" re-runs Step 7 but preserves user-modified files (test by manually editing CLAUDE.md between runs, then choosing refresh — your edits should NOT be silently overwritten; you should get an overwrite/skip/abort prompt per file)

---

## 8 — Conflict-detection test

In a fresh folder, install Spec-Kit manually first:

```
mkdir C:\scratch\mck-test-conflict
cd C:\scratch\mck-test-conflict
specify init test-project --ai claude
claude
/mck-scaffold:init
```

**Answers** that would route to MUSUBI (e.g., Q1=c, Q2=greenfield, Q3=team10, Q4=mand).

**Pass criteria:**
- Step 8 detects existing `.specify/` marker before attempting MUSUBI install
- Conflict prompt offers: install alongside / skip this install / abort
- Default = skip
- Scenario completes cleanly with MUSUBI not installed; final summary documents the conflict and the skip

---

## 9 — Audit log template

Create `docs/audit-log.md` on first failure. Use this format per finding:

```markdown
## YYYY-MM-DD — Scenario X — short title

**Repro:** {what you did}

**Expected:** {what should have happened per pass criteria}

**Actual:** {what actually happened}

**Hypothesis:** {what you suspect is wrong}

**Workaround / fix applied:** {if any}

**Files touched:** {list}
```

Group findings by scenario. Don't try to fix as you go — capture, then revisit in priority order after the full sweep.

---

## 10 — What "Phase 9 done" looks like

- [ ] All 5 install + 4 scenario tests run (Scenarios A, B, C, D)
- [ ] Re-run tests (§7) run on at least one scenario folder
- [ ] Conflict-detection test (§8) run
- [ ] `docs/audit-log.md` exists, even if empty
- [ ] Top 3 issues from the audit log captured as GitHub issues (when the repo goes public) OR as TODOs in `DESIGN.md §10 OPEN`

If 80% of pass criteria across all scenarios pass, mck-scaffold ships v1.0.
If <80%, the failing scenarios become Phase 7.1 / 7.2 iteration targets before v1.0.

---

## 11 — Known limitations (already documented; not bugs)

- **Slash-command installs are surfaced as manual steps**, not run automatically. This is by design (skills can't invoke slash commands programmatically). Re-document if pass criteria are unclear.
- **Hook arrays in settings.json are empty.** Phase 7 left them empty pending exact Claude Code hook schema verification. Stack/methodology hook content lands as Phase 7.1 iteration.
- **Version pinning is `@latest`** for upstream installs. Phase 7.1 will pin to known-good once vetted.
- **No automated CI / unit tests** for the scaffolder itself. Dogfood IS the test loop in v1. Automated tests are a v2 consideration.
