# Interview questions

The Q1–Q8 tree that `/mck-scaffold:init` walks the user through. Asked one at a time, with smart pruning (brownfield skips Q3–Q4).

**Status:** Phase 1 stub. Question wording is canonical; the interview engine that consumes this file ships in Phase 2.

---

## Q1. What is the audit / compliance posture for this project?

- **(a) None.** Never asked. No external audit risk.
- **(b) Sometimes.** Internal compliance review, occasional questions.
- **(c) Always.** External regulator, recurring audits.
- **(d) Security-critical.** Fintech, healthcare, automotive embedded, or similar where security defects have direct user/financial impact.

**Routes:** (a)→L1 candidate. (b)→L2 candidate. (c)→L2 + CSDD overlay. (d)→L2 + CSDD overlay + L3-watch flag.

---

## Q2. Is this a new project (greenfield) or an existing codebase (brownfield)?

- **(a) Greenfield.** New repo, nothing to integrate with.
- **(b) Brownfield.** Existing codebase, adding features or refactoring.

**Routes:** (b)→Context C (OpenSpec + Shotgun CLI), skips Q3 and Q4.

---

## Q3. How many people will work on this code?

*(Skipped if Q2 = brownfield.)*

- **(a) Solo.** Just me, possibly with occasional collaborators.
- **(b) Small team (2–9).** Shared conventions matter; coordination overhead is real.
- **(c) Team (10+).** Formal process, shared standards required.

**Routes:** (c) + Q1 ≥ b → L2 candidate confirmed.

---

## Q4. How strict is your TDD discipline?

*(Skipped if Q2 = brownfield.)*

- **(a) Mandatory.** Production code without a failing test first is a blocker.
- **(b) Encouraged.** Tests for new logic but not blocking.
- **(c) Off / per-feature.** Will decide per change.

**Routes:** (a)→Superpowers HARD-GATE (L1) or MUSUBI Article III (L2). (b)→Spec-Kit (L1). (c)→lean preset.

---

## Q5. What stack(s) is this project using? *(multi-select)*

- .NET / C#
- Angular / TypeScript
- Unity / game dev
- Python
- Mixed / multi-stack
- Other (free text)

**Routes:** Determines which `templates/stack/<name>/` adapters get composed in.

---

## Q6. Is an MCP gateway available for this project?

*(Default auto-set from Q3: team → yes; solo → no. User can override.)*

- **(a) Yes.** Bifrost or another gateway is configured.
- **(b) No.** Plain CLI tools (`gh`, curl, grep) only.

**Routes:** (a)→MCP allowlist template. (b)→CLI orientation.

---

## Q7. What is your multi-agent appetite?

*(Capped per project constraint: subagent-orchestration platforms are out of v1 scope.)*

- **(a) None.** Single Claude session per task.
- **(b) Limited.** 2–3 explicit subagents max (planner, reviewer, etc.).

---

## Q8. Show me the L3-watch document so I know what to monitor?

*(Only asked if Q1 = d.)*

- **(a) Yes.** Include `docs/horizon/L3-WATCH.md` with Tessl / CSDD status + re-eval cadence.
- **(b) No.** Skip.

---

## Pruning rules summary

- Q2 = brownfield → skip Q3, Q4 (route straight to OpenSpec)
- Q1 = a AND Q3 = solo → default Q6 to (b) CLI
- Q1 ≥ c AND Q3 = team10+ → default Q6 to (a) MCP
- Q1 ≠ d → skip Q8
