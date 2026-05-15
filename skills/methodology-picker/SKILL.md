---
name: methodology-picker
description: Use this skill when the user asks to "set up a new project", "start a new project", "scaffold a project", "pick a methodology", "choose an SDD framework", "configure Claude Code for this project", "what methodology should I use", or discusses agentic development methodology selection. Helps the user route to the right Spec-Driven Development approach based on Piskala's three-level rigor taxonomy and Wasowski's four decision contexts. Does NOT write files — conversational guidance only. For actual scaffolding, route to /mck-scaffold:init.
version: 0.1.0
---

# Methodology Picker

Help the user pick the right Spec-Driven Development methodology for their project context. This is conversational guidance, not scaffolding. The user is making a decision; you are helping them frame it.

## When this skill auto-triggers

The user mentions:
- Starting / setting up / scaffolding a new project
- Choosing / picking / selecting an SDD framework or methodology
- Configuring Claude Code for a specific project
- "What methodology should I use for …"
- Comparison of Superpowers vs Spec-Kit vs MUSUBI vs OpenSpec
- Piskala / Wasowski by name

## What to NOT do

- Do not write files. If the user wants files written, route them to `/mck-scaffold:init`.
- Do not install plugins. Same rule.
- Do not present all options as equivalent — the whole point is to *choose*.

## Conceptual frame

### Piskala's three-level rigor taxonomy

| Level | Promise | Cost | When to pick it |
|---|---|---|---|
| **L1 Spec-First** | Flexibility | Low | ~80% of projects. Spec lives in the feature branch and becomes historical after merge. |
| **L2 Spec-Anchored** | Auditability + traceability | Medium-high | Regulated work. Spec lives the whole lifecycle; every change requires an addendum. |
| **L3 Spec-as-Source** | Guarantee by construction | Aspirational in 2026 | Watch / not-yet. Tessl (closed beta) and CSDD (academic). |

**Higher rigor is not better.** It is more expensive, and only pays off when the project genuinely needs the promise.

### Wasowski's four decision contexts

| Context | Profile | Default path |
|---|---|---|
| **A** | Solo / small team, greenfield | Spec-Kit lean ± Superpowers HARD-GATE |
| **B** | 10+ team, regulated, compliance | MUSUBI + CSDD security overlay |
| **C** | Brownfield / legacy | OpenSpec + Shotgun CLI |
| **D** | Security-critical (fintech, healthcare, embedded) | CSDD + MUSUBI + OWASP Skill |

## How to talk the user through it

Ask questions one at a time. Don't batch. Reference the question tree in `interview/questions.md` but adapt the wording conversationally.

The single most-discriminating question is: **"Will an auditor ever ask about REQ-to-Test traceability for this code?"** Answer determines L1 vs L2 cleanly.

Second-most-discriminating: **"Greenfield or brownfield?"** Brownfield routes to OpenSpec almost regardless of other answers.

Third: **"Solo / small team / 10+?"** Solo + greenfield + no audit → L1-lean or L1-superpowers. Team 10+ + audit → L2.

## Honest caveats to surface

When recommending Superpowers: note the HN consensus that "I stripped it back to ~30%" — it's opinionated; that's a feature for serious work, friction for a 20-minute fix.

When recommending Spec-Kit: note the measured ~10x agent-time overhead on the first cycle (33.5 min vs 8 min baseline). Worth it on real product work, painful on throwaways.

When recommending MUSUBI: note that 28 GitHub stars vs Superpowers' 166k is not a sign of MUSUBI being bad — it's a sign that most teams don't need its rigor. If an auditor will ask, the 28 stars are irrelevant.

When the user's context suggests L3 might matter in 18+ months (security-critical financial/healthcare): mention Tessl and CSDD as "watch, not deploy" — don't oversell them. Tessl is closed beta; CSDD is a paper with one empirical study.

## Pointer to the scaffolder

If at any point the user says "okay, do it" / "set this up" / "scaffold it" — route them to:

```
/mck-scaffold:init
```

…which runs the full interview and writes the files. This skill is the conversation; the slash command is the action.

## Status

Phase 2 complete. Conceptual frame, conversational guidance flow, and routing references are all functional. The interview executor (`/mck-scaffold:init`) consumes the same `interview/questions.md` and `interview/routing.md` files this skill references — they are the single source of truth for both the conversational and the deterministic paths.
