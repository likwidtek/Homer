# Project guidance

Before doing product design or implementation work, read these files in order:

1. `docs/PRODUCT.md`
2. `docs/DECISIONS.md`
3. `docs/SESSION.md`

Treat only explicitly confirmed decisions as requirements. Keep proposals, assumptions, and unresolved questions clearly labeled; do not resolve them silently. Record consequential accepted decisions in `docs/DECISIONS.md`, deferred ideas in `docs/ROADMAP.md`, and update `docs/SESSION.md` at a natural handoff point.

Decky is the initial distribution path, but the Homer agent must remain an independently managed runtime. Do not expose arbitrary shell access through the phone client; privileged operations must be fixed, audited actions with deliberate confirmation.

Build to maximum Decky Plugin Store safety compliance even while initial releases are delivered through GitHub. Keep code concise, explainable, documented, tested, and manually reviewable. Do not add self-updaters, arbitrary remote code installation, controller-input hijacking, or broad destructive behavior.

Do not write application code until the user has approved an implementation plan.
