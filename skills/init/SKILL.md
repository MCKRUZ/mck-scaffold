---
name: init
description: Initialize a new Claude Code project with a tailored Spec-Driven Development methodology. Runs an interview to determine the right approach (L1 Spec-First / L2 Spec-Anchored / L3-watch), composes templates, writes the project's CLAUDE.md / AGENTS.md / .claude/settings.json / decision docs, and triggers upstream framework installs (Superpowers, Spec-Kit, MUSUBI, OpenSpec). Invoked via /mck-scaffold:init.
argument-hint: [--dry-run] [--overlay=<name>]
allowed-tools: [Read, Write, Edit, Glob, Grep, AskUserQuestion, Bash]
version: 0.7.0
---

# /mck-scaffold:init

> **Status:** Phase 7 — fully functional. Runs the interview (Steps 1–6), then executes scaffolding (Step 7), triggers upstream installs (Step 8), and prints the final summary (Step 9). Acceptance: same answers always produce the same target-project files; re-runs are idempotent.

## Pipeline overview

```
Step 1: detect project context
Step 2: load interview definition (questions.md + routing.md)
Step 3: run interview (Q1–Q8 with pruning)
Step 4: apply routing (deterministic lookup)
Step 5: produce decision summary
Step 6: confirm or iterate
─────────── confirmation point ───────────
Step 7: execute scaffolding (compose + write files)
Step 8: trigger upstream installs
Step 9: final summary + manual-steps callout
```

Steps 1–6 do not write to the target project. Steps 7–8 do.

---

## Step 1 — detect project context

Run these checks in the current working directory and report what you found:

- Does `.git/` exist?
- Does `CLAUDE.md` exist?
- Does `.claude/` exist?
- Do any SDD framework markers exist? Check for `.specify/`, `docs/constitution.md` with MUSUBI banner, `openspec/`, `docs/superpowers/`, `~/.claude/plugins/.../superpowers/`.
- Does `docs/decisions/INTERVIEW.md` exist? **This is the re-run indicator.**

Print a one-line context summary, e.g.:

> Detected: `.git` present, no existing CLAUDE.md, no prior SDD framework. Treating this as a **fresh init in an existing repo**.

### Re-run handling

If `docs/decisions/INTERVIEW.md` is present, route to the re-run subroutine (see "Idempotency" near the end). Do NOT proceed with a fresh interview when a prior scaffold record exists.

---

## Step 2 — load the interview definition

Read these files from the plugin root (the directory containing `.claude-plugin/plugin.json`):

- `interview/questions.md` — Q1–Q8 question tree, options, pruning rules
- `interview/routing.md` — answer-combination → preset routing table + install command list

These are the source of truth. If their content conflicts with anything in this SKILL.md body, the markdown files win.

---

## Step 3 — run the interview

For each question Q1–Q8 (respecting pruning):

1. Read the question wording and options from `interview/questions.md`.
2. Build an `AskUserQuestion` call using the verbatim wording.
3. Q5 is `multiSelect: true`; all others single-select.
4. For Q6: set the auto-default per Q3 (solo → CLI, team10+ → MCP) as the first option, but always ask.
5. Apply pruning before each next question:
   - After Q2: if `brownfield`, skip Q3 and Q4.
   - After Q1: if `≠ d`, skip Q8.
6. One question per `AskUserQuestion` call. Never batch.

Record answers in a running dictionary.

---

## Step 4 — apply routing

Read `interview/routing.md`. Match answers against the primary routing table — this is a deterministic lookup, **not** a judgment call. Resolve:

- `methodology_preset`: one of `L1-lean`, `L1-spec-kit`, `L1-superpowers`, `L2-musubi`, `L2-openspec-brownfield`
- `methodology_overlays`: zero or more of `L2-csdd-security`, `L3-watch`
- `stack_adapters`: from Q5 (each selected stack name, e.g., `dotnet`, `angular`, `unity`, `python`)
- `mcp_orientation`: yes/no from Q6
- `multi_agent_appetite`: none/limited from Q7
- `install_commands`: from routing.md's "Meta-installer triggers per preset" table — collect commands for preset + each overlay

If the routing table has no exact match for the answer combination, that's a routing bug. Report it to the user and abort — do not silently pick a default.

---

## Step 5 — produce the decision summary

Print the markdown block defined in Phase 2 (interview answers table, resolved methodology, rationale paragraphs, scaffolded-files preview, honest caveats from `methodology-picker/SKILL.md`). Do not write to disk yet.

---

## Step 6 — confirm or iterate

After the decision summary, ask via `AskUserQuestion`:

> "Does this routing look right?"
> - **Yes — proceed to scaffold and install.** → continue to Step 7
> - **No — try a different preset.** → surface 2 next-closest routing rows; user picks one; re-render summary
> - **Tweak an answer.** → ask which question to revisit; rerun forward from there
> - **Dry-run only.** → skip Steps 7-8; print "what would be written / installed" without writing

If `--dry-run` was passed as an argument, force the dry-run path.

---

## Step 7 — execute scaffolding

> **Order matters.** Compose files first (Steps 7a–7g), THEN run installs (Step 8). Some overlay operations (e.g., CSDD `constitution.append.md`) must happen AFTER the methodology's install creates the base constitution — those are handled at the end of Step 8.

Define the variable dictionary first:

```
{{project_name}}            = basename of cwd
{{project_path}}            = absolute cwd
{{scaffold_date}}           = today (ISO format YYYY-MM-DD)
{{mck_scaffold_version}}    = from .claude-plugin/plugin.json "version"
{{re_eval_date}}            = scaffold_date + 6 months
{{git_present}}             = yes/no
{{prior_claude_md}}         = yes/no
{{prior_claude_dir}}        = yes/no
{{prior_sdd_markers_or_none}} = comma-separated detected markers or "(none)"
{{methodology_preset}}      = from Step 4
{{methodology_overlays_or_none}} = csv or "(none)"
{{methodology_overlays_suffix}}  = " with overlays X, Y" or ""
{{preset_one_liner}}        = first sentence of methodologies/{preset}/README.md
{{stack_adapters_csv}}      = csv from Q5
{{mcp_orientation}}         = yes/no
{{multi_agent_appetite}}    = none/limited
{{q1_answer}} ... {{q8_answer}} = verbatim option labels from questions.md
{{q5_answer_csv}}           = csv of selected stacks
{{pruning_notes}}           = e.g. "Q2 was 'brownfield', so Q3 and Q4 were skipped." or "No pruning applied."
{{user_notes_or_none}}      = free-text "Other" notes from interview or "(none)"
{{rationale_paragraphs}}    = generate from preset + answers (1-3 short paragraphs)
{{alternatives_list}}       = next-closest routing rows as bulleted "not chosen because..." list
{{caveats_block}}           = caveats from methodology-picker that apply to this preset
{{scaffolded_files_list}}   = filled in as Step 7 progresses
{{installed_plugins_list}}  = filled in as Step 8 progresses
```

### 7a — read all template fragments

From the plugin root:

- `templates/shared/CLAUDE.md.tmpl`
- `templates/shared/AGENTS.md.tmpl`
- `templates/shared/settings.json.tmpl`
- `templates/shared/docs/decisions/INTERVIEW.md.tmpl`
- `templates/shared/docs/decisions/METHODOLOGY.md.tmpl`
- `templates/methodologies/{methodology_preset}/CLAUDE.append.md`
- `templates/methodologies/{methodology_preset}/settings.merge.json`
- For each overlay: `templates/methodologies/{overlay}/CLAUDE.append.md`, `settings.merge.json`, and `constitution.append.md` if present
- For each stack in `stack_adapters`: `templates/stack/{stack}/CLAUDE.append.md`, `settings.merge.json`

Cache content in memory before any writes.

### 7b — compose CLAUDE.md

1. Start with shared `CLAUDE.md.tmpl`, substituted.
2. Build `{{methodology_specific_content}}` = methodology preset's `CLAUDE.append.md`, substituted. Append any overlay's `CLAUDE.append.md`, substituted, in routing order.
3. Build `{{stack_specific_content}}` = concatenation of each stack adapter's `CLAUDE.append.md`, in Q5 selection order, each substituted.
4. Substitute the two content blocks into the shared template.
5. Verify no `{{...}}` placeholders remain. If any do, report a substitution gap and abort the write.

Write to `{{project_path}}/CLAUDE.md`. See "Step 7h — write contract" for conflict handling.

### 7c — compose AGENTS.md

Substitute variables on `AGENTS.md.tmpl`. Write to `{{project_path}}/AGENTS.md`.

### 7d — compose `.claude/settings.json`

Start with `settings.json.tmpl`. Deep-merge each layer's `settings.merge.json` in order: methodology → overlays → each stack (in Q5 selection order).

Merge rules:
- **Arrays**: append. For `permissions.allow` and `permissions.deny`, deduplicate.
- **Objects**: deep-merge recursively.
- **Scalars**: later wins.

Apply MCP overlay if `mcp_orientation = yes` (Phase 7 placeholder: leave hooks arrays as merged; MCP-specific allowlist fragment lands when MCP overlay template is built — currently a no-op).

Write the merged JSON to `{{project_path}}/.claude/settings.json`.

### 7e — write `docs/decisions/INTERVIEW.md`

Substitute the shared `INTERVIEW.md.tmpl` with answers and project context. Write to `{{project_path}}/docs/decisions/INTERVIEW.md`.

### 7f — write `docs/decisions/METHODOLOGY.md`

Build `{{rationale_paragraphs}}`, `{{alternatives_list}}`, `{{caveats_block}}` per the variable dictionary. `{{scaffolded_files_list}}` is the running list of files written so far. `{{installed_plugins_list}}` will be filled after Step 8.

Substitute and write to `{{project_path}}/docs/decisions/METHODOLOGY.md`. This file is updated again after Step 8 to fill in `{{installed_plugins_list}}`.

### 7g — L3-watch overlay

If `L3-watch` is in `methodology_overlays`, write `templates/methodologies/L3-watch/horizon/L3-WATCH.md` (substituted) to `{{project_path}}/docs/horizon/L3-WATCH.md`. (The L3-watch template ships in Phase 8.)

### 7h — write contract

For each file about to be written:

| Situation | Action |
|---|---|
| Target file does not exist | Write. Add to `scaffolded_files_list`. |
| Target file exists, content matches what we would write | Skip silently. No-op. |
| Target file exists, content differs | Show a unified diff. AskUserQuestion: **overwrite** / **skip** / **abort the whole scaffold**. Default = skip. |

Never silently overwrite a file the user has changed. Mck-scaffold is opinionated about respecting user edits.

---

## Step 8 — trigger upstream installs

Read the `install_commands` resolved in Step 4. There are two command types:

### 8a — bash-runnable commands

Examples: `specify init {{project_name}} --ai claude`, `npx musubi-sdd@latest init`, `uvx shotgun plan "<short description>"`, `openspec init`.

For each:

1. **Conflict pre-check.** Detect existing markers:
   - Spec-Kit: `.specify/` already exists → conflict
   - MUSUBI: `docs/constitution.md` with MUSUBI banner + `.claude/skills/@orchestrator` → conflict
   - OpenSpec: `openspec/` already exists → conflict
2. **If conflict found**, AskUserQuestion:
   - **Install alongside** — proceed; the upstream tool will likely fail or warn, surface its output
   - **Skip this install** — note in summary, continue
   - **Abort entire scaffold** — stop everything (files already written remain; document this in summary)
   - **Default: skip this install**
3. **No conflict** → run the command via `Bash` tool with output captured.
4. For Shotgun: prompt user for the `"<short description>"` argument before running — it's free-text.
5. **Verify success** — check expected post-install markers exist (`.specify/`, `openspec/`, etc.). If markers missing, log the failure and continue with the next install. Don't abort the whole run for a partial install.

### 8b — slash-command installers

`/plugin install …` is a Claude Code harness command, not a Bash command. A skill body cannot programmatically invoke another slash command. Surface them to the user explicitly:

```
⚠ Manual step required:
The following slash commands must be run in this Claude Code session to complete the install.
Please run each one and confirm before continuing:

  /plugin install superpowers@claude-plugins-official
  /plugin reload
```

Capture in `installed_plugins_list` regardless (with a `(manual)` marker). Trust that the user ran them.

### 8c — version pinning (policy)

Per `DESIGN.md §10`, the locked default is "pin to known-good; quarterly re-eval." In Phase 7, we do NOT yet hard-pin upstream versions — install commands use `@latest` or the marketplace's chosen version. Pinning lands as a Phase 7.1 iteration once known-good versions for each upstream are vetted.

### 8d — CSDD constitution append (post-install)

If `L2-csdd-security` is in `methodology_overlays`:

1. Wait until `docs/constitution.md` exists in the target project (MUSUBI's `npx musubi-sdd init` creates it).
2. Read `templates/methodologies/L2-csdd-security/constitution.append.md` (substituted).
3. Append it to the end of `{{project_path}}/docs/constitution.md`.
4. Add the file to `scaffolded_files_list` as "modified".

If MUSUBI's install didn't create `docs/constitution.md` (failure, skipped, conflict), skip the append and surface a warning.

---

## Step 9 — final summary

Re-substitute `METHODOLOGY.md` to fill in `{{installed_plugins_list}}`.

Print a closing markdown block:

```markdown
✅ mck-scaffold complete.

## Files written

{scaffolded_files_list — relative paths}

## Plugins / commands installed

{installed_plugins_list — including `(manual)` slash-command items}

## Manual steps still required

{slash commands the user must run, if any — usually `/plugin install superpowers@...` + `/plugin reload`}

## Re-evaluation cadence

This methodology is recommended for review on **{{re_eval_date}}** (six months from today). At that point, re-run `/mck-scaffold:init` — it will detect this scaffold and offer reconfigure or diff.

## Next

1. Read `CLAUDE.md` — it's your project's authoritative conventions.
2. Read `docs/decisions/METHODOLOGY.md` — it's why this setup was chosen.
3. {preset-specific next step, e.g.:
   - L1-superpowers: "Try `/superpowers:brainstorm` on your first feature."
   - L1-spec-kit: "Try `/speckit.specify` on your first feature."
   - L2-musubi: "Try `@requirements-analyst` to draft your first EARS spec."
   - L2-openspec-brownfield: "Run `uvx shotgun plan` again if your codebase changes significantly."
}
```

---

## Idempotency — re-run handling

When `docs/decisions/INTERVIEW.md` exists at Step 1:

1. Read the prior answers and the prior `methodology_preset` from the file.
2. AskUserQuestion:
   - **Reconfigure from scratch** — re-run Steps 2–9 with fresh answers (this overwrites INTERVIEW.md and METHODOLOGY.md)
   - **Refresh templates with same answers** — re-run Steps 7–9 only; useful when mck-scaffold has been updated and you want the new template content
   - **Show diff vs current state** — iterate through what would be written, output a unified diff per file, no writes happen
   - **Abort**
3. **Default: abort.** Re-runs are destructive; require explicit user opt-in.

---

## Argument handling

- `--dry-run` — force the dry-run path at Step 6 (skip Steps 7–8 entirely)
- `--overlay=<name>` — Phase 10 feature; Phase 7 echoes "(overlay support arrives in Phase 10)" and continues normally

---

## Acceptance criteria (Phase 7)

- [ ] Phase 2 criteria still pass (interview is deterministic, no files in Steps 1–6)
- [ ] Step 7 composes CLAUDE.md / AGENTS.md / settings.json correctly per the shared → methodology → overlays → stacks order
- [ ] No `{{...}}` placeholders remain in any target-project file after substitution
- [ ] Existing target files are never overwritten without explicit user confirmation
- [ ] Step 8 runs bash-runnable installs and surfaces slash-command installs as manual steps
- [ ] Conflict detection fires before each install
- [ ] CSDD constitution append happens AFTER MUSUBI's `docs/constitution.md` is created
- [ ] Re-run on a folder with prior INTERVIEW.md never silently overwrites
- [ ] `--dry-run` skips Steps 7–8 cleanly
- [ ] Final summary lists every file written and every install triggered (including `(manual)` items)

---

## Implementation notes for Claude (not user-facing)

- Use `Write` and `Edit` for file ops. `Write` creates parent dirs automatically.
- For variable substitution, do a straightforward string replace per variable in the variable dictionary. Don't try to be clever with regex — templates are authored by humans.
- For deep-merge of JSON: implement as nested object walk. Arrays append; dedup `permissions.allow` and `permissions.deny`; objects merge recursively; scalars overwrite.
- For `Bash` installs:
  - Run with reasonable timeouts (60–180s typically; Shotgun on large repos may need 600s).
  - Capture both stdout and stderr; surface stderr if non-empty.
  - Don't use `--no-confirm` flags speculatively — only when documented by the upstream tool.
- If a Bash install fails, continue with the next install. Log the failure for the final summary. Don't abort the whole scaffold on one upstream failure.
- Cache the plugin root path (containing `.claude-plugin/plugin.json`) at the start of Step 1. All template loads are relative to it.
- For the diff-display path (re-run option c, and the per-file conflict resolution): use a clear unified-diff format (`--- old`, `+++ new`, hunks). Don't print giant files; truncate at ~200 lines and link to the source paths.
