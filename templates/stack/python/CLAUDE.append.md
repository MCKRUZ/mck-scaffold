### Stack: Python

#### Naming

- `snake_case`: functions, variables, modules.
- `PascalCase`: classes, type aliases.
- `UPPER_SNAKE_CASE`: constants, module-level config.

#### Style

- Type hints on **every** function signature (including private). `from __future__ import annotations` for forward references.
- Prefer immutable data: `frozenset`, `tuple`, frozen `@dataclass`. Mutate at well-defined boundaries only.
- `Path` from `pathlib`, never raw strings for file paths.
- f-strings, never `%` or `.format()`.

#### Project layout

```
project/
├── pyproject.toml         # uv + ruff config
├── src/
│   └── package_name/
│       ├── __init__.py
│       └── ...
└── tests/
    └── ...
```

`src/` layout, not flat. Tests outside the package directory.

#### Testing

- **pytest** only. Not unittest.
- **Fixtures over setup methods.** `conftest.py` for cross-file fixtures.
- **`@pytest.mark.parametrize`** for variant cases — avoids repetitive test functions.
- Test naming: `test_scenario_expected_result(...)`.
- Coverage: `pytest --cov=src --cov-report=term-missing`.

#### Tooling

- **`uv`** for project, venv, and dependency management. Not pip / poetry / pipenv.
- **`ruff`** for lint + format. One tool, not three.
- Useful commands:
  - `uv sync` — install deps from `pyproject.toml`
  - `uv add <pkg>` — add a dependency
  - `uv run pytest` — run tests in the project venv
  - `ruff check` — lint
  - `ruff format` — auto-format
  - `ruff check --fix` — auto-fix what's safely fixable

#### Error handling

- Custom exception types for domain errors. Inherit from a project-specific base (`MyAppError(Exception)`).
- Never `except Exception:` without re-raising or logging with stack trace.
- Use `logging` (not `print`) for production code. Configure once at entry point.

#### Async

- `asyncio` for I/O concurrency. Don't mix `threading` and `asyncio` carelessly.
- `async def` functions need `await` somewhere; if they don't, they shouldn't be async.

#### Dependency hygiene

- `uv pip list --outdated`
- `uv pip audit` (or `pip-audit` if integrated separately)
- Pin major versions in `pyproject.toml`. Review changelogs before upgrading.
