# mck-scaffold — Audit Log

Findings captured during dogfood runs of `/mck-scaffold:init`. Format per `TEST_PLAN.md` §9. Most recent first.

---

## 2026-05-15 — Dogfood #1 — consultant-helper (Wasowski Context A-ish, leans L1)

**Repro:** Manual walk-through of the init pipeline against `C:\Users\RP325UG\OneDrive - EY\Documents\GitHub\MCKRUZ\consultant-helper`. Acted as the init skill body (Steps 1–7) since slash-command invocation from a different CC session isn't possible. Answers: Q1=b, Q2=greenfield, Q3=solo, Q4=enc, Q5=.NET+Angular, Q6=no(CLI), Q7=limited. Resolved preset: `L1-spec-kit` + dotnet + angular adapters.

**Expected:**
- Step 1 detects pre-existing `.claude/`, `design-reference/`, absence of `.git`/CLAUDE.md/SDD-markers
- Q8 pruned because Q1≠d
- Routing deterministic, lands on L1-spec-kit (not L2) per `Q1=b + solo + Q4=enc` rule
- 5 composed files written, no `{{...}}` placeholders leak
- Existing files preserved

**Actual:** All pass criteria met. Five files written cleanly, grep for `{{` returned zero matches, design-reference and settings.local.json preserved.

**Findings worth flagging:**

1. **No git-init concern.** Target had no `.git/` and mck-scaffold made no attempt to create one (correct per spec — out of v1 scope). But worth surfacing in `METHODOLOGY.md` as a "you'll probably want this" prompt rather than burying it. Candidate for v1.1: an "init git? (y/n)" question late in the interview.

2. **`.claude/settings.local.json` distinction was correctly preserved**, but only by coincidence — the spec doesn't explicitly call out the local-vs-shared settings distinction. If a target had `.claude/settings.json` already (not just `.local.json`), Step 7h's diff-prompt would fire. Document the distinction explicitly in init/SKILL.md to remove the implicit assumption.

3. **`design-reference/` folder was preserved but never explicitly examined for routing implications.** It contains HTML mockups that strongly suggest "consulting workflow tool" — the LLM running the interview could have surfaced this as context to the user ("I see mockups suggesting X — does that match what you're building?"). Currently the interview is stateless about pre-existing assets beyond marker files. Candidate for v1.1: light pre-interview "I see X in the folder, does that affect your answers?" prompt.

4. **Manual `specify init` step is friction.** The dogfood deferred to "you run this separately" rather than running it via Bash. In a real `/mck-scaffold:init` run (Phase 7+8 properly), this would have been a Bash call. Validating the actual `specify init` execution remains untested.

5. **Variable substitution worked cleanly** — no edge cases observed. Particularly: `{{mcp_orientation}}` = "no" rendered as `**MCP orientation:** no` in METHODOLOGY.md, which reads naturally despite being a yes/no value. No formatting concerns.

6. **Composition order verified by inspection** — methodology section appears before stack sections in CLAUDE.md (correct per spec). Stack sections concatenated in Q5 selection order (.NET first, then Angular, matching the user's multi-select).

**Hypothesis:** Templates are sound. The five candidates for v1.1 iteration are: (a) git-init prompt, (b) explicit settings.local-vs-shared documentation in init/SKILL.md, (c) pre-existing-assets context prompt, (d) actual Step 8 install execution validation, (e) hook arrays (still empty — Phase 7.1 issue, not a finding from this dogfood).

**Workaround / fix applied:** None during this run. Findings logged for Phase 7.1 / 7.2 / v1.1 batch.

**Files touched:** Target — wrote `consultant-helper/{CLAUDE.md, AGENTS.md, .claude/settings.json, docs/decisions/{INTERVIEW.md, METHODOLOGY.md}}`. Scaffold repo — only this audit-log.md, nothing else changed.
