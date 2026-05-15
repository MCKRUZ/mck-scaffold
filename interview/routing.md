# Routing — answer combination → preset

How Q1–Q8 answers map to a methodology preset name. The preset name resolves to a folder under `templates/methodologies/`.

**Status:** Phase 1 stub. Logic is canonical; the routing engine that consumes this file ships in Phase 2.

---

## Primary routing table

Answers are abbreviated: Q1 ∈ {a,b,c,d}; Q2 ∈ {green,brown}; Q3 ∈ {solo,small,team10}; Q4 ∈ {mand,enc,off}. `*` means "any value".

| Q1 | Q2 | Q3 | Q4 | Preset(s) |
|---|---|---|---|---|
| a | green | solo | mand | `L1-superpowers` |
| a | green | solo | enc | `L1-spec-kit` |
| a | green | solo | off | `L1-lean` |
| a | green | small | mand | `L1-superpowers` |
| a | green | small | enc | `L1-spec-kit` |
| a | green | small | off | `L1-lean` |
| a | green | team10 | * | `L1-spec-kit` |
| b | green | * | mand | `L2-musubi` (lite mode) |
| b | green | * | enc \| off | `L1-spec-kit` *(L1 sufficient when audit is intermittent)* |
| c | green | * | * | `L2-musubi` + `L2-csdd-security` overlay |
| d | green | * | * | `L2-musubi` + `L2-csdd-security` overlay + `L3-watch` overlay |
| * | brown | * | * | `L2-openspec-brownfield` *(plus Shotgun CLI install trigger)* |

## Stack composition

For every preset row above, the resolved scaffold is:

```
templates/shared/                        ← base files always written
+ templates/methodologies/<preset>/      ← from routing table
+ templates/methodologies/<overlay>/...  ← any overlays from routing table
+ templates/stack/<stack>/               ← from Q5 (one per multi-select value)
```

Files are merged with shared as the base; methodology fragments append; stack fragments are last (most specific wins).

## MCP/CLI orientation (Q6)

- (a) MCP → add `templates/shared/settings.mcp-allowlist.json` fragment
- (b) CLI → default settings.json (no MCP config)

## Multi-agent (Q7)

- (a) None → no subagent definitions in scaffold
- (b) Limited → add 2–3 subagent stubs (`planner.md`, `reviewer.md`) into `.claude/agents/`

## L3 watch (Q8)

- Only triggers if Q1 = d AND Q8 = (a).
- Adds `templates/methodologies/L3-watch/horizon/L3-WATCH.md` to the scaffold.
- Never installs anything; pure informational doc.

## Meta-installer triggers per preset

Captured here in canonical form. Step 8 of `init/SKILL.md` **auto-executes** these — they are not optional manual steps for the user.

### L1-lean

(none)

### L1-spec-kit

Windows (auto-detected by Step 8 when `$IsWindows` or equivalent):

```powershell
$env:PYTHONIOENCODING = "utf-8"
$env:PYTHONUTF8 = "1"
uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude --force --script ps
```

macOS / Linux:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude --force --script sh
```

**Drift notes (verified 2026-05-15 via dogfood):**
- `--ai` is deprecated in current Spec-Kit. Use `--integration`.
- `--force` is required to skip the "directory not empty" confirm prompt when mck-scaffold has already scaffolded the base CLAUDE.md / AGENTS.md / settings.json.
- `--script ps` (Windows) / `--script sh` (Unix) sets the shell-script variant.
- `PYTHONIOENCODING=utf-8` + `PYTHONUTF8=1` on Windows fixes a `cp1252` `UnicodeEncodeError` in Spec-Kit's startup banner. Banner crashes before any init logic runs without this.
- Slash command names changed: dot-format (`/speckit.X`) → hyphen-format (`/speckit-X`).
- Commands moved location: `.claude/commands/speckit.*.md` → `.claude/skills/`.
- Spec-Kit now auto-`git init`s by default (this becomes opt-in via `specify extension add git` after v0.10.0).
- Spec-Kit's startup output recommends adding parts of `.claude/` to `.gitignore` to prevent agent-credential leakage. Step 7 of init should surface this; see L1-spec-kit/README.md.

### L1-superpowers

```
/plugin install superpowers@claude-plugins-official
/plugin reload
```

These are **slash-command installs** — a skill body cannot programmatically invoke `/plugin install`. Step 8b surfaces these to the user with a "run this now" manual-step instruction. The user types the commands; we trust-but-verify by checking `~/.claude/plugins/.../superpowers/` afterward.

### L2-musubi

```
npx musubi-sdd@latest init
```

Auto-confirms via `--yes` flag if MUSUBI supports it (verify on first refresh; currently treated as interactive — auto-pipe `y` if needed).

### L2-openspec-brownfield

```
uvx shotgun plan "<user-provided short description>"
openspec init
```

Step 8a prompts the user for the short description before invoking Shotgun.

### L2-csdd-security (overlay)

No install. Templates only — appends `constitution.append.md` content to `docs/constitution.md` after `L2-musubi`'s install creates the base constitution.

### L3-watch (overlay)

No install. Informational doc only — writes `docs/horizon/L3-WATCH.md`.

## Conflict handling

If the target project already has an SDD framework installed (detected by presence of `.specify/`, `.musubi/`, `.openspec/`, or `~/.claude/plugins/.../superpowers/`):

1. Surface the conflict to the user with the slash-command equivalent of an `AskUserQuestion`.
2. Offer (a) install alongside, (b) abort and let them choose manually, (c) overwrite with diff preview.
3. Default behavior is (b).

## Idempotency rules

When `/mck-scaffold:init` runs in a folder with prior `docs/decisions/INTERVIEW.md`:

1. Read prior answers.
2. Offer (a) reconfigure from scratch, (b) show diff against current state, (c) abort.
3. Files locally modified since scaffold are NEVER overwritten without explicit per-file confirm.
