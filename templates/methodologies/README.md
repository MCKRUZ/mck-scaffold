# templates/methodologies/

Per-methodology fragments. Each preset is a small folder that contributes additive fragments to the shared base.

| Subfolder | Phase | Family |
|---|---|---|
| `L1-lean/` | 4 | L1 Spec-First (minimum) |
| `L1-spec-kit/` | 4 | L1 Spec-First (GitHub Spec-Kit) |
| `L1-superpowers/` | 4 | L1 Spec-First (obra Superpowers) |
| `L2-musubi/` | 5 | L2 Spec-Anchored (MUSUBI core) |
| `L2-openspec-brownfield/` | 5 | L2 Spec-Anchored (OpenSpec for legacy) |
| `L2-csdd-security/` | 5 | L2 overlay (CSDD security clauses) |
| `L3-watch/` | 8 | L3 informational doc only |

Composition rule: shared base + methodology + overlays + stack. Methodology contributes fragments named by purpose (`CLAUDE.append.md`, `settings.merge.json`, `docs/...`) — the scaffolder composes them.
