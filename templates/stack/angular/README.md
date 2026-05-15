# stack/angular

> Angular / TypeScript stack adapter. Lifted from the user's `~/.claude/rules/{coding-style,testing,security}.md`.

## What this adapter contributes

- `CLAUDE.append.md` — immutability (spread/NgRx), error handling (catchError + logger), Reactive Forms validation, testing (Jasmine + HttpTestingController + afterEach verify), E2E (Playwright + `data-testid`), XSS posture
- `settings.merge.json` — `permissions.allow` entries for `ng:*`, `npm:*`, `npx playwright:*`; hooks empty until Phase 7

## When composed

Q5 = `Angular / TypeScript` (alone or multi-select). Layered on top of any methodology preset. Often paired with `dotnet` adapter for full-stack projects.

## Assumes

- Angular 18+ (signals, standalone components)
- NgRx (or signals-based equivalent) for non-trivial state
- Jasmine + Karma (not Jest)
- Playwright (not Cypress) for E2E
