### L1-lean — methodology notes

This project uses the **L1-lean** preset: Spec-First discipline with no upstream framework.

**The rule that matters:** for any non-trivial change, enter Plan Mode (`Shift+Tab` twice), iterate the plan until it's right, *then* implement. Skip Plan Mode at your own risk.

**No artifact trail.** This preset doesn't ship a spec format or template. If you want one, copy something from your own past project, or escalate to `L1-spec-kit` for a structured constitution + spec/plan/tasks pipeline.

**No mandatory TDD.** Tests when the change warrants them; not as a blocking gate. If you want TDD enforced, escalate to `L1-superpowers`.

**Escape valves:**
- Found yourself skipping Plan Mode repeatedly → re-run `/mck-scaffold:init` and pick `L1-superpowers` (HARD-GATE) or `L1-spec-kit` (constitution).
- Auditor materialized → re-run and route to L2-musubi.
