# stack/python

> Python stack adapter. Lifted from the user's `~/.claude/rules/{coding-style,testing}.md`.

## What this adapter contributes

- `CLAUDE.append.md` — naming, type hints, testing (pytest + fixtures + parametrize), tooling (ruff + uv)
- `settings.merge.json` — `permissions.allow` entries for `python:*`, `pytest:*`, `ruff:*`, `uv:*`, `pipx:*`

## When composed

Q5 = `Python` (alone or multi-select).

## Assumes

- Python 3.12+
- `uv` for project / venv management (not `pip` / `poetry` / `pipenv`)
- `ruff` for lint + format (not `black` + `flake8` + `isort`)
- `pytest` for testing (not `unittest`)
