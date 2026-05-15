### L1-spec-kit — methodology notes

This project uses **GitHub Spec-Kit** (L1 Spec-First). The constitution and templates live in `.specify/`.

**The 5-command pipeline** — run these in order for any non-trivial feature:

1. `/speckit.constitution` — review the constitution if you haven't recently. It defines what is and isn't allowed.
2. `/speckit.specify` — write the spec. Spec lives in the feature branch.
3. `/speckit.plan` — produce the implementation plan.
4. `/speckit.tasks` — break the plan into T001..T0NN tasks with priority and parallelism markers.
5. `/speckit.implement` — execute. The CLI will refuse to run this without an existing spec.md, plan.md, and tasks.md.

**The constitution is the contract.** When code conflicts with an article, the article wins. If you find yourself wanting to violate an article, that's a signal to amend the constitution explicitly — don't silently route around it.

**Articles to remember:**
- Article I — Library-First Principle: features start as standalone libraries before being inlined.
- Article III — Test-First Imperative: tests precede production code (advisory in Spec-Kit; if you want this enforced, escalate to `L1-superpowers`).
- Article VI — Simplicity Gate: reject the more abstract solution unless complexity is justified.

For the full articles, see `.specify/memory/constitution.md`.

**When Spec-Kit gets in the way:**
- One-line fixes don't need the pipeline. Use judgment.
- The first-cycle overhead is ~10x baseline. That's the cost of the artifact trail.
