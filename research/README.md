# research/

> The catalog refresh pipeline. Tracks the upstream agentic-coding ecosystem so mck-scaffold's routing logic doesn't ossify against a snapshot of 2026-05-15.

## What lives here

| File | Purpose |
|---|---|
| `last-run.json` | Timestamp + summary of the most recent research run. **Source of truth** for "how stale is the catalog?" |
| `findings.md` | Consolidated narrative of current ecosystem state. Human-readable synthesis. |
| `changelog.md` | Deltas over time. Every refresh appends an entry. |
| `streams/github-landscape.md` | Structured facts from the GitHub repo survey (framework names, stars, install commands). |
| `streams/reddit-signal.md` | Practitioner-signal patterns from Reddit + aggregator sources. |
| `streams/youtube-tutorials.md` | Tactics from creator tutorials that affect routing. |
| `streams/web-blogs.md` | Authoritative blog / engineering writeup citations. |

## How a refresh works

1. User runs `/mck-scaffold:init` → Step 0 reads `last-run.json` and offers refresh based on staleness.
2. User opts in (or runs `/mck-scaffold:refresh-research` directly).
3. `skills/research-refresh/SKILL.md` orchestrates 4 parallel research subagents (GitHub / Reddit / YouTube / web-blogs).
4. Each stream returns updated structured facts.
5. Pipeline diffs new findings against current `streams/*.md` files.
6. User reviews the diff per-section (per Phase 12 design: **show diff, require confirm** — no silent updates).
7. Confirmed changes apply to `routing.md`, methodology preset READMEs, and the stream files themselves.
8. `last-run.json` timestamp updated; `changelog.md` appended with the delta summary.

## Why this exists

The dogfood on `consultant-helper` (2026-05-15) immediately surfaced that Spec-Kit had drifted from what the Wasowski 2026 article described — `--ai` deprecated, slash commands renamed dot→hyphen, commands moved to skills. **That was less than 30 days after the article published.** The agentic-coding ecosystem moves faster than annual re-evals can track. Without a built-in refresh mechanism, mck-scaffold rapidly becomes a snapshot of when it was written rather than a live tool.

The shared-repo store (vs local-only) means: one user runs the refresh → commits → everyone benefits. Refreshes are essentially contributions to the plugin.

## Staleness thresholds

`last-run.json` includes a `days_since` field that init's Step 0 uses to decide tone:

- **< 30 days:** "Last refresh: N days ago. Catalog is fresh — skipping by default." (Skip path; user can override.)
- **30–90 days:** "Last refresh: N days ago. Consider refreshing — ecosystem may have moved." (Neutral prompt.)
- **> 90 days:** "Last refresh: N days ago. **Strongly recommended** — drift is likely." (Refresh-by-default prompt.)

## What this does NOT do (yet — v0.11.0 scope)

- **v0.11.0 (current):** research-refresh produces a human-readable diff. The user manually applies template changes via Edit.
- **v0.11.1 (planned):** auto-apply confirmed diff sections to template files. Reduces manual-edit burden but requires the diff format to be machine-parseable per change.
- **v0.12 (planned):** scheduled background refresh (e.g., monthly) via a hook or cron-equivalent.
