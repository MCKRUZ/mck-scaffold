# Stream: youtube-tutorials

> Last refreshed: 2026-05-15
> Method: Creator-survey + companion-blog cross-reference (YouTube transcript MCP was unavailable during baseline; tactic extraction relied on creators' own blog posts).

## High-credibility creators (real shipped projects, real Claude Code usage)

### Anthropic-affiliated

- **Cal Rueb (Anthropic engineer)** — "Claude Code best practices | Code w/ Claude" (2025). Authoritative on plug-in / hook / settings.json design. The Boris-credited contributor to the official best-practices doc.
- **Boris Cherny @ Sequoia AI Ascent 2026** — "Why Coding Is Solved." Creator of Claude Code. Personal workflow: 10-15 parallel sessions as "workers"; treats sessions as a job queue. Plan Mode = 2-3x success rate claim.
- **"Two Anthropic engineers" 24-min walkthrough** (Apr 2026, multiple URLs circulating) — comprehensive feature tour. Heavy social virality.

### Indie / high-signal

- **IndyDevDan** — "Multi-Agent Orchestration with Opus 4.6, Tmux and Agent Sandboxes" (Feb 9, 2026). Power-user perspective. **Delegate mode** (Shift+Tab on lead) as the #1 multi-agent failure prevention.
- **Cole Medin** — Founder of Dynamous; Plan → Implement → Validate loop with explicit validation pipeline (lint, tests, type-check).
- **Net Ninja** — "Spec Driven Workflow with Claude Code" 4-part series (Feb 2026). Build `/spec` custom slash command. Mandatory Plan Mode toggle. "Spec is the source of truth — fix the spec, not chat history."
- **alexop.dev / Alex Opalic** — "Spec-Driven Development with Claude Code in Action" (Feb 2026). EARS-format requirements; `Explore` subagent type with Haiku for read-only scans; `isolation: worktree` on subagents for parallel edits.
- **CampusX** — "Spec-Driven Development in Claude Code" (Apr 2026). Beginner-to-intermediate Spec-Kit walkthrough.

### Heavyweight methodology walkthroughs

- **Brian (BMad creator)** — "The Official BMad-Method Masterclass (The Complete IDE Workflow)" (Aug 2025). Definitive walkthrough. Two-phase (Agentic Planning → Context-Engineered Development). Scrum Master shards PRD + architecture so Dev agents only load what they need.

## Consensus tactics (3+ creator independent confirmation)

These tactics appeared in multiple unrelated creator videos:

1. **Plan Mode before code.** Universal — Cherny, Cal Rueb, IndyDevDan, Net Ninja, alexop, Cole Medin, BMad.
2. **One task per session + `/clear` at ~60% context.** Universal — context "drops off a cliff" around 40-50k tokens.
3. **Subagents for context isolation, not just parallelism.** Spawn fresh-context research agents in a single message; converge reports.
4. **Lean CLAUDE.md (~100 lines).** Anthropic + alexop + Cole Medin agree.
5. **Spec/PRD as durable file.** Common to SDD, BMAD, Spec-Kit.
6. **Independent verification subagent for TDD.** Anthropic best-practices + alexop + Cole Medin.
7. **Observability mandatory for multi-agent.** IndyDevDan's hard line.
8. **Press Escape early when Claude drifts.** Quiet but universal.

## Anti-patterns surfaced

1. Starting with 30 agents (IndyDevDan).
2. Lead agent doing tasks itself instead of delegating — fix with delegate mode.
3. Long chat sessions without `/clear` ("vibe coding chaos").
4. Trusting subagent self-tests without independent verification.
5. `--dangerously-skip-permissions` outside sandboxed/no-internet container.
6. Treating skills as shortcuts rather than progressive-disclosure context.

## What to verify on next refresh

- YouTube transcript MCP should be retested — direct transcript extraction is more reliable than creators' companion blogs.
- New videos from Anthropic engineers on advanced topics (skills authoring, hook design, MCP integration).
- BMAD v6 release walkthroughs (in active development at baseline).

## Sources

(URLs preserved at baseline; verify on next refresh that they still resolve)

- IndyDevDan multi-agent: `youtube.com/watch?v=RpUTF_U4kiw`
- Cal Rueb best practices: `youtube.com/watch?v=gv0WHhKelSE`
- Boris Cherny @ Sequoia: `youtube.com/watch?v=SlGRN8jh2RI`
- Two Anthropic engineers 24-min: `youtube.com/watch?v=x7m_Mr14KIc` and `youtube.com/watch?v=a0_B2C9jsYY`
- BMad masterclass: `youtube.com/watch?v=LorEJPrALcg`
- Net Ninja /spec series #1: `youtube.com/watch?v=e_D9M_MJ9Hs`
- Cole Medin channel: `youtube.com/@ColeMedin`
