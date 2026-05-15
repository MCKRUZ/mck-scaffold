# stack/unity

> Unity / game development stack adapter. Lifted from the user's `~/.claude/rules/game-dev.md`.

## What this adapter contributes

- `CLAUDE.append.md` — Assembly Definitions, ScriptableObjects, prefab-first workflow, `[SerializeField] private`, MonoBehaviour vs pure C#, object pooling, events over direct references, performance budget, shader notes, NVIDIA integration, Play Mode testing
- `settings.merge.json` — minimal; Unity tooling has poor CLI integration so most discipline is informational, not hook-enforced

## When composed

Q5 = `Unity / game dev`. Usually solo (you specifically have Unity hobby projects).

## Assumes

- Unity 6+ (or 2023 LTS)
- Universal Render Pipeline (URP) or High Definition Render Pipeline (HDRP)
- C# only (no UnityScript)
- NVIDIA RTX Kit available locally if relevant (`D:\NVIDIA\RTX-Kit\`)
