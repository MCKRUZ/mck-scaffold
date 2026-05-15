# stack/dotnet

> .NET / C# stack adapter. Lifted from the user's `~/.claude/rules/{coding-style,testing,security}.md`.

## What this adapter contributes

- `CLAUDE.append.md` — immutability, file organization, naming, Result\<T\>, FluentValidation + MediatR, DI patterns, project defaults, xUnit + WebApplicationFactory testing
- `settings.merge.json` — `permissions.allow` entries for `dotnet:*` and `nuget:*`; hooks left empty until Phase 7 locks the exact schema

## When composed

Q5 = `.NET / C#` (alone or multi-select). Layered on top of any methodology preset.

## Assumes

- .NET 8 or 9 (modern minimal API + records + init-only properties)
- xUnit (not NUnit / MSTest)
- FluentValidation + MediatR pipeline behavior
- EF Core for data access (raw SQL only via `FromSqlInterpolated`)
