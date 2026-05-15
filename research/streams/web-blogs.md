# Stream: web-blogs

> Last refreshed: 2026-05-15
> Method: Direct WebFetch + WebSearch on authoritative engineering blogs, Anthropic engineering articles, and academic papers.

## Anthropic engineering canon (foundational, read these first)

| Article | Why it matters |
|---|---|
| [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) (Schluntz + Zhang, Dec 2024) | Five composable workflow patterns (prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer). Foundational. Every framework is implementing some subset of these. |
| [How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) (2025) | Orchestrator-worker beat single-agent by 90.2% on internal research evals — primarily because separate context windows aggregate more total tokens. **~15x token cost** caveat. |
| [Best practices for Claude Code](https://www.anthropic.com/engineering/claude-code-best-practices) (Boris Cherny, 2025) | The Explore → Plan → Code → Commit loop. Subagents in `.claude/agents/`. Writer/Reviewer multi-session pattern. |
| [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (Sep 2025) | "Context rot" formalized. Just-in-time retrieval via lightweight identifiers > upfront dumping. |
| [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) (2025) | "Context anxiety" — premature wrap-up on long horizons. Reset > compaction alone. Initializer agent + `init.sh` + `claude-progress.txt`. |
| [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) (Oct 16, 2025) | Skills as filesystem-based, progressively-disclosed capability bundles. 3-tier disclosure. Open standard at `agentskills.io` (Dec 18, 2025). |
| [How Anthropic teams use Claude Code](https://www.anthropic.com/news/how-anthropic-teams-use-claude-code) (2025) | Most internal value from non-coding teams using Claude as a thought partner. |

## Independent practitioner essays (contrarian + foundational)

| Author | Article | Key thesis |
|---|---|---|
| Armin Ronacher (Sentry / Flask) | [Agentic Coding Recommendations](https://lucumr.pocoo.org/2025/6/12/agentic-coding/) (Jun 2025) | **Skip MCP for most things.** Plain CLI tools are sufficient. Sonnet > Opus for most work. "yolo mode" (`--dangerously-skip-permissions`) inside containers/VMs. |
| Armin Ronacher | [Agent Design Is Still Hard](https://lucumr.pocoo.org/2025/11/21/agents-are-hard/) (Nov 2025) | SDK abstractions break under real tool use. Cache management + reinforcement + failure isolation do more work than the model. |
| Simon Willison | [Agentic Engineering Patterns](https://simonwillison.net/2026/Feb/23/agentic-engineering-patterns/) (Feb 2026) | Agentic Engineering ≠ vibe coding. Pattern-language format inspired by GoF. |
| Geoffrey Litt | Malleable Software essays (Jun 2025) | End-user authored, hackable personal software. "LLMs writing code to be called by future LLM sessions." HUDs over Copilots. |
| Hamel Husain + Shreya Shankar | [Evals Skills for Coding Agents](https://hamel.dev/blog/posts/evals-skills/) (2026) | Coding agents are now strong enough to do their own evals. End-to-end task success + step-level diagnostics. Skip generic metrics (BERTScore, ROUGE). |

## Production engineering case studies

| Source | Article / blog | Takeaway |
|---|---|---|
| Bessemer / Farhan Thawar (Shopify VP Eng) | "Inside Shopify's AI-first engineering playbook" | Standardize infrastructure not tooling. LLM gateway pattern. ~20% productivity gain in exploration breadth, not LOC. |
| Vercel | Vercel blog (2026) | 30%+ deployments agent-initiated; Claude Code ~75% of those. Open-sourced `deepsec` security harness. |
| Sourcegraph | Sourcegraph engineering | Claude Code's workspace-only context is the binding constraint at enterprise scale; expose code search via MCP. |
| Stripe / Upstash | "Ship full-stack app with Stripe Projects" | End-to-end provisioning from agent terminal — secrets stay in vault, never in chat scrollback. |

## Critical / skeptical pieces (counter-balance)

| Source | Article | Thesis |
|---|---|---|
| Fortune (Apr 2026) | "Engineering missteps were behind Claude Code's monthlong decline" | Late-2025 quality crisis was real. Routing bugs + throttling + "stealth price hikes." User loyalty fragile under silent behavior changes. |
| InfoWorld (2026) | "Enterprise developers question Claude Code's reliability for complex engineering" | 60-80% failure rates on execution-heavy multi-step tasks (SWE-EVO 2025). Enterprises route hard problems elsewhere. |
| arXiv 2603.20847 | Engineering Pitfalls in AI Coding Tools (empirical, 3.8K+ bugs) | **Tool invocation (37.6%) + command execution (25%) = 62.6% of bugs.** The model is fine; the harness is where failures live. |
| Robert Matsuoka | "When Claude Forgets How to Code" (late 2025) | Context-collapse layer operates silently. Mitigation: start fresh, verify negative claims with your own search. |
| Check Point | CVE-2025-59536 / CVE-2026-21852 | Repo-level `.claude/` configs enable RCE on clone-and-open. **Audit `.claude/` before opening untrusted repos.** |

## SDD-specific anchor (Wasowski synthesis)

- [Comparing 15 Spec-Driven Development Frameworks](https://medium.com/@jaroslaw.wasowski) (Wasowski, Apr 2026) — THE canonical comparison. mck-scaffold's routing is directly traceable to this article + Piskala + Marri.
- [Piskala 2026](https://arxiv.org/abs/2602.00180) — three-level rigor taxonomy
- [Marri 2026](https://arxiv.org/abs/2602.02584) — Constitutional SDD; 73% security defect reduction (banking microservices, single study)

## What to verify on next refresh

- Have Anthropic engineering articles been refreshed? Check for new entries since "Equipping agents for the real world with Agent Skills" (Oct 2025).
- Has Tessl exited closed beta? Has it published Eval Framework results publicly?
- Have the arXiv 2603.20847 findings (tool-invocation as primary bug source) been replicated or extended?
- Has Shopify or Vercel published updated agentic-deployment metrics?

## Sources

All URLs preserved in-line above; verify on next refresh that they still resolve.
