# research/changelog.md

> Deltas in the framework catalog over time. Every research-refresh appends an entry here. Most recent first.

---

## 2026-05-15 — Baseline + Spec-Kit drift fixes

**Refresh kind:** baseline (initial research capture) + dogfood-surfaced drift fixes

### Frameworks tracked (initial)

- **L1 Spec-First (11):** Superpowers, Spec-Kit, BMAD-METHOD, GSD, cc-sdd, Don Cheli SDD, Agent OS v3, Shotgun CLI, WordPress SDD, OWASP Security Skill, plus L1-lean (mck-scaffold's own minimal preset)
- **L2 Spec-Anchored (3):** MUSUBI, Intent (Augment Code, closed), OpenSpec
- **L3 Spec-as-Source (2):** Tessl (closed beta), CSDD (academic)

### Source basis

Wasowski 2026 (Medium, "Comparing 15 SDD Frameworks") + Piskala 2026 (arXiv:2602.00180, three-level rigor taxonomy) + Marri 2026 (arXiv:2602.02584, Constitutional SDD) + Anthropic engineering articles + 4-stream research sweep run on 2026-05-15.

### Drift detected during dogfood (consultant-helper)

| Framework | Field | Before | After | Notes |
|---|---|---|---|---|
| Spec-Kit | install flag | `--ai claude` | `--integration claude` | `--ai` deprecated, will be removed in v0.10.0 |
| Spec-Kit | slash command syntax | `/speckit.X` (dot) | `/speckit-X` (hyphen) | All 5 core + 3 enhancement skills |
| Spec-Kit | command location | `.claude/commands/speckit.*.md` | `.claude/skills/` | Migrated to modern skills format |
| Spec-Kit | git init | manual user step | auto by default | Will become opt-in in Spec-Kit v0.10.0 |
| Spec-Kit | Windows banner | n/a | crashes on cp1252 codepage | Workaround: set `PYTHONIOENCODING=utf-8` + `PYTHONUTF8=1` |
| Spec-Kit | new enhancement skills | n/a (not surfaced) | `/speckit-clarify`, `/speckit-analyze`, `/speckit-checklist` | Added to L1-spec-kit/CLAUDE.append.md |

### Files updated in this delta

- `interview/routing.md` — install commands modernized; Windows / Unix split
- `templates/methodologies/L1-spec-kit/CLAUDE.append.md` — slash commands renamed + skills location + enhancement skills added
- `templates/methodologies/L1-spec-kit/README.md` — drift notes table, install commands platform-split, security advisory
- `skills/init/SKILL.md` Step 8a — auto-install explicit, platform detection, env var setup, post-install security-advisory surfacing

### Validated via dogfood

- Spec-Kit init succeeded with `--integration claude --force --script ps` + UTF-8 env vars set
- Spec-Kit non-destructively appended `<!-- SPECKIT START -->` block to mck-scaffold's CLAUDE.md (no overwrite)
- Spec-Kit auto-`git init`ed the project (free win)

### Notable findings preserved for future diffs

- Superpowers star count: ~166k (Wasowski Apr 2026) — was ~93k from secondary-source Anthropic marketplace listing (May 2026 confirmed). Either is plausible; growth post-marketplace inclusion is real.
- HN consensus on Superpowers: "I stripped it back to ~30%" — opinionated, costs friction.
- Reddit aggregator-only signal: primary Reddit MCP was down during baseline; signal is second-hand via Morph LLM / AI Tool Discovery / HumanLayer / etc. **Next refresh should re-attempt primary Reddit access.**
