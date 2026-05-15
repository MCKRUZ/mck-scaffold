# templates/stack/

Stack-specific adapters layered on top of methodology presets. Each adapter contributes a `CLAUDE.append.md` (conventions appended to target's `CLAUDE.md`) and a `settings.merge.json` (hooks / permissions merged into target's `.claude/settings.json`).

## Available adapters

| Subfolder | Stack | Source |
|---|---|---|
| `dotnet/` | .NET 8/9, C#, xUnit, FluentValidation, MediatR, EF Core | Lifted from `~/.claude/rules/{coding-style,testing,security}.md` |
| `angular/` | Angular 18+, TypeScript, NgRx, Jasmine/Karma, Playwright | Same |
| `unity/` | Unity 6 / 2023 LTS, URP/HDRP, MonoBehaviour + pure C#, Play Mode tests | Lifted from `~/.claude/rules/game-dev.md` |
| `python/` | Python 3.12+, `uv`, `ruff`, `pytest` | Lifted from `~/.claude/rules/{coding-style,testing}.md` |

## Multi-stack ("mixed") composition

There is **no** `mixed/` subfolder. When Q5 is multi-select, the scaffolder composes the chosen adapters in order:

1. Pick all selected stack subfolders.
2. Concatenate each adapter's `CLAUDE.append.md` (in selection order).
3. Deep-merge each adapter's `settings.merge.json` into the shared base settings.
4. Deduplicate `permissions.allow` / `deny` arrays.
5. Append all hook entries — no automatic conflict resolution; if two adapters configure the same hook event, the later-selected adapter wins.

Common combinations:
- `.NET / C#` + `Angular / TypeScript` → typical full-stack project
- `Python` + `Angular / TypeScript` → Python backend, Angular frontend
- `Unity` alone is almost always the right call for game projects

## What this directory does NOT contain

- **Frontend frameworks other than Angular** (React, Vue, Svelte). Out of v1 scope per the user's actual stack. Add via fork or post-v1 contribution.
- **Mobile** (iOS / Android / React Native). Out of v1 scope.
- **Infra-as-code** (Terraform, Bicep, Pulumi). Out of v1 scope.
- **ML / data science notebooks**. Out of v1 scope.

## Composition order recap

When the scaffolder builds the target project's `CLAUDE.md`:

1. `templates/shared/CLAUDE.md.tmpl` (base, variable-substituted)
2. `templates/methodologies/<preset>/CLAUDE.append.md` (substituted into `{{methodology_specific_content}}` slot)
3. Each `templates/methodologies/<overlay>/CLAUDE.append.md` (appended for each overlay)
4. Each `templates/stack/<chosen-stack>/CLAUDE.append.md` (appended into `{{stack_specific_content}}` slot, in selection order)

Same composition order for `settings.json` (each layer's `settings.merge.json` merged on top).
