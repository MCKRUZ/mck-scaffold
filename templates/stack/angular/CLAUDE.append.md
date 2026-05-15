### Stack: Angular / TypeScript

#### Immutability

- Never mutate objects or arrays directly. Use spread: `{ ...obj, key }` and `[...arr, item]`.
- NgRx reducers: always return new state objects. No `state.foo = bar`.
- Signals (Angular 18+): use `.update()` with a new value, not `.set()` with mutation of the current.

#### File organization

- One component per file. Components and their templates colocated.
- kebab-case file names (`user-profile.component.ts`).
- Services in `core/` or feature folders; never put logic in components.

#### Naming

- PascalCase: types, classes, interfaces, components.
- camelCase: variables, functions, observables (suffix `$` for observables).
- kebab-case: file names.

#### Error handling

- Use `catchError` in RxJS pipes. Never let errors silently die.
- Use a centralized logger service. **No `console.log` in production code.**
- User-facing error notification on caught errors (toast / dialog).

#### Validation

- Reactive Forms only. No template-driven forms for non-trivial input.
- Validators composed from `Validators.required`, `Validators.pattern`, etc. + custom validators in a shared `validators/` folder.
- Show validation errors only after `touched || dirty` — never on initial render.

#### Testing

- Jasmine + Karma. Descriptive `it('should ...')` strings.
- HTTP: `HttpTestingController` with `afterEach(() => httpMock.verify())`. Verify-after-each is non-negotiable — without it, leaks across tests are silent.
- Run: `ng test --code-coverage`, `ng test --watch=false --browsers=ChromeHeadless` for CI.

#### E2E

- Playwright (not Cypress / WebdriverIO).
- Selectors: `data-testid="..."` only. Never CSS selectors. Never XPath. The component can refactor; the test should not break.
- Run: `npx playwright test`.

#### Security

- XSS: Angular auto-escapes by default. Use `DomSanitizer` **only when unavoidable**, with an inline comment explaining why.
- Never put secrets in client code. Backend proxies all external API calls.
- CSRF protection enabled on all state-changing endpoints (handled in the backend, but the frontend must send the cookie).

#### Dependency hygiene

Before adding:

```
npm audit
npm outdated
```

Pin major versions. Review changelogs before upgrading.
