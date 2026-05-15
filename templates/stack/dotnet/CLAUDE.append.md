### Stack: .NET / C#

#### Immutability

- Public surfaces: `IReadOnlyList<T>` / `IReadOnlyDictionary<K,V>`. Never expose mutable collections.
- Private backing fields: `readonly` arrays.
- DTOs / config: init-only properties.
- Value types: records. **Not** for polymorphic hierarchies — use classes with factory methods there.

#### File organization

- Clean Architecture with type-organized layers (`Exceptions`, `Extensions`, `Interfaces`, `Models`, `Services`) — not feature folders within layers.
- One class per file. Most files under 150 lines, hard cap 400.
- Each layer ships a `DependencyInjection.cs` with `Add{Layer}Dependencies()` extension methods.

#### Naming

- PascalCase: classes, methods, properties.
- `_camelCase`: private fields.
- camelCase: locals, parameters.
- `I` prefix: interfaces.

#### Error handling

- `Result<T>` for expected failures (validation, auth, business rules). Factory methods: `Result<T>.Success(value)`, `Result<T>.Fail(message)`, `Result.ValidationFailure(errors)`.
- Exceptions **only** for truly exceptional conditions.
- Structured JSON logging. Never swallow errors silently.

#### Validation

- FluentValidation on all DTOs. Auto-discovered via assembly scanning. Applied through MediatR pipeline behavior.
- Parallel validation with `Task.WhenAll` where independent.

#### Dependency injection

- Keyed DI (`AddKeyedSingleton` / `AddKeyedTransient`) for extensible registrations (tools, connectors, providers).
- MediatR pipeline behavior **order matters** — document registration order in `DependencyInjection.cs`.

#### Project defaults

```xml
<PropertyGroup>
  <ImplicitUsings>enable</ImplicitUsings>
  <Nullable>enable</Nullable>
</PropertyGroup>
```

Configuration: `IOptionsMonitor<T>` with strongly-typed config hierarchies. Options pattern, not raw `IConfiguration`.

#### Testing

- xUnit. Arrange-Act-Assert. Moq sparingly.
- Integration tests: `WebApplicationFactory<Program>` + in-memory DB (EF Core `UseInMemoryDatabase` or SQLite in-memory).
- Test naming: `MethodName_Scenario_ExpectedResult` (e.g., `Handle_InvalidRequest_ReturnsValidationFailure`).
- Commands: `dotnet test` for unit, `dotnet test --collect:"XPlat Code Coverage"` for coverage runs.

#### Security

- Secrets: User Secrets for dev (`dotnet user-secrets set "Key" "value"`), Azure Key Vault for prod. Never in source.
- SQL: EF Core only (auto-parameterized). Raw SQL via `FromSqlInterpolated` only — never string concat.
- JWT: `ValidateLifetime=true`, `ClockSkew=Zero`, validate issuer + audience + signing key.
- CORS: explicit allowlist (`CorsAllowedOrigins`), never wildcard in production.

#### Vulnerability checks

Before adding a dependency:

```
dotnet list package --vulnerable
dotnet list package --outdated
```

Pin major versions. Review changelogs before upgrading.
