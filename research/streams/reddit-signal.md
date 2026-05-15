# Stream: reddit-signal

> Last refreshed: 2026-05-15
> Method: **Degraded** — primary Reddit MCP server was unreachable during baseline; synthesis is second-hand from aggregator sources (Morph LLM, AI Tool Discovery, HumanLayer, ClaudeKit, Claude Codex). Flagged for next refresh to re-attempt direct Reddit access.

## Consensus tactics observed across multiple aggregator-summarized threads

These are HIGH-CONFIDENCE because they appeared in 3+ unrelated aggregator summaries citing different threads:

1. **Plan Mode universal.** Boris Cherny (Claude Code creator) on r/ClaudeAI: matches his own personal workflow.
2. **Save plans to `./plans/*.md` and pass paths between subagents** (not content). Reduces context cost dramatically.
3. **Auto-delegation to subagents fails in practice.** Multiple ClaudeKit and r/ClaudeCode reports — Claude rarely summons subagents automatically. **Workaround: list available subagents explicitly in CLAUDE.md with delegation guidance.** This is one of the strongest "marketed-but-broken" signals.
4. **CLAUDE.md target <150 lines, achievable <60.** HumanLayer hard data. Reasoning: Claude Code system prompt already burns ~50 instructions; instruction following degrades past ~150-200.
5. **Hooks for deterministic, CLAUDE.md for advisory.** Same pattern across many threads.
6. **`/clear` at ~60% context.** Context degrades silently past ~40-50k tokens per most threads cited.

## Frameworks discussed (signal on real usage)

| Framework | Reddit signal | Sentiment |
|---|---|---|
| Superpowers | Heavy discussion; "stripped it back to 30%" top HN comment (cited in aggregator) | Opinionated; loved by some, friction for others |
| Spec-Kit | "Lowest learning curve but stale PR queue and no community channel" | Pragmatic baseline |
| BMAD-METHOD | "Trap for two-person startups; impressive when deployment target is SOC 2 audit" | Heavy ceremony; right tool wrong context most common failure |
| MUSUBI | Not widely discussed (28 stars matches signal) | Niche, audit-driven |
| OpenSpec | "Highest score in one head-to-head test (same feature, Cursor + Sonnet 4.5)" but one-maintainer | Bus-factor risk |

## Failure patterns / "things that don't work as marketed"

1. **Automatic subagent delegation** (see consensus tactic #3)
2. **`--dangerously-skip-permissions` outside containers** — runaway agent risk
3. **Subagent self-tests without independent verification** — overfits to its own tests
4. **Long marathon sessions** — token economics get worse 2-3x for "fix forward" vs "restart fresh"
5. **Claude Code weekly limit hit in 3 days** — root cause "reading large files to answer a question about one function, generating test files 80% boilerplate"

## What to verify on next refresh

- Re-attempt primary Reddit MCP access. Aggregator signal is directionally reliable but weak on attribution (no thread URLs, no upvote counts, no author handles).
- Has the "Claude quality regression" sentiment cycle (cf. Sept 2025 "Claude Is Dead" thread, 841 upvotes) recurred? Anthropic quality cycles drive Reddit sentiment more than methodology debates.

## Sources

- Morph LLM: "Claude Code Reddit: What Developers Actually Say (2026)"
- AI Tool Discovery: "Claude Code Reddit: What Developers Actually Use It For in 2026"
- ClaudeKit: "Claude Code Subagents: Common Mistakes & Best Practices"
- HumanLayer: "Writing a good CLAUDE.md"
- dev.to / Will Torber: "Spec Kit vs BMAD vs OpenSpec: Choosing an SDD Framework in 2026"
- ranthebuilder.cloud: "I Tested Three Spec-Driven AI Tools. Here's My Honest Take."
- Claude Codex: methodologies-ecosystem
- self.md: "Plan Mode Mastery"
