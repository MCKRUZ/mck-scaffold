# Stream: github-landscape

> Last refreshed: 2026-05-15
> Method: GitHub repo survey + install-command extraction. Cross-checked against Anthropic example-plugin + locally-installed marketplace manifests (Superpowers, context-mode).

## Framework facts (current — 2026-05-15)

| Name | Family | Stars | Install | Status | Drift since prior run |
|---|---|---|---|---|---|
| Superpowers (obra/superpowers) | L1 | ~166k (Wasowski Apr 2026); ~93k (Anthropic marketplace listing) | `/plugin install superpowers@claude-plugins-official` | Active | n/a (baseline) |
| Spec-Kit (github/spec-kit) | L1 | ~90k | `uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude --force --script {ps|sh}` | Active | **DRIFT:** `--ai` deprecated → `--integration`; `/speckit.X` → `/speckit-X`; commands moved to `.claude/skills/`; auto-`git init` default; Windows banner crashes without `PYTHONIOENCODING=utf-8` + `PYTHONUTF8=1` |
| BMAD-METHOD (bmad-code-org) | L1 (heavy) | ~45k | `npm i -g @bmad-code-org/bmad` | Active, v6 in dev | n/a |
| GSD | L1 | ~48k | (TBD) | Active | n/a |
| OpenSpec | L2 (lightest) | ~5.8k | `openspec init` | Active, one-maintainer bus-factor risk | n/a |
| cc-sdd | L1 | ~1.5k | `npx cc-sdd init` | Active | n/a |
| Don Cheli SDD | L1 | n/a | TypeScript orchestrator (TBD) | Active, 93+ commands | n/a |
| Agent OS v3 | L1 (enhancer) | n/a | (TBD) | Active, layers on Spec-Kit | n/a |
| Shotgun CLI | L1 (brownfield prep) | n/a | `uvx shotgun plan "..."` | Active | n/a |
| WordPress SDD Constitution | L1 (domain-specific) | n/a | (TBD) | Active | n/a |
| OWASP Security Skill | L1 (overlay) | ~145 | (TBD) | Active | n/a |
| MUSUBI | L2 | 28 | `npx musubi-sdd@latest init` | Active, niche | n/a |
| Intent (Augment Code) | L2 | $252M funded, closed | (commercial) | Active commercial | n/a |
| Tessl | L3 | $125M funded, closed beta | (closed beta — no public install) | Watch | n/a |
| CSDD (Marri 2026) | L3 | academic (arXiv:2602.02584) | (reference paper only) | Watch | n/a |

## Cross-cutting GitHub findings

- **`commands/` directory is legacy** in Claude Code plugin format. Modern plugins use `skills/<name>/SKILL.md` for both auto-triggered skills AND user-invoked slash commands (frontmatter distinguishes). Documented in `~/.claude/plugins/marketplaces/claude-plugins-official/plugins/example-plugin/README.md`.
- **Plugin manifest schema**: `name` + `description` required; `version`, `author`, `homepage`, `repository`, `license`, `keywords` optional. Commands/skills discovered by directory convention. Verified against installed manifests.
- **Marketplace manifest schema**: two patterns observed —
  - Single-plugin local: `source: "./"` in `marketplace.json` colocated with `plugin.json` (context-mode pattern, used by mck-scaffold for github-installable form)
  - Multi-plugin remote: each plugin entry has `source: {source: "url", url: "..."}` (superpowers-marketplace pattern)

## What to verify on next refresh

- Has Superpowers' star count converged on the Wasowski ~166k figure or the ~93k secondary source figure? (Resolve by direct GitHub view.)
- Has Spec-Kit's v0.10.0 shipped? If so, `git extension` requires explicit opt-in.
- Any new L2 frameworks emerged?
- Has Tessl exited closed beta?

## Sources

- Wasowski 2026, "Comparing 15 Spec-Driven Development Frameworks" (Medium)
- GitHub repos for each framework listed
- Local Claude Code plugin install at `~/.claude/plugins/`
