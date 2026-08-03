# Project guidance

Before doing product design or implementation work, read these files in order:

1. `docs/PRODUCT.md`
2. `docs/DECISIONS.md`
3. `docs/SESSION.md`

Treat only explicitly confirmed decisions as requirements. Keep proposals, assumptions, and unresolved questions clearly labeled; do not resolve them silently. Record consequential accepted decisions in `docs/DECISIONS.md`, deferred ideas in `docs/ROADMAP.md`, and update `docs/SESSION.md` at a natural handoff point.

## Task routing and documentation

Read `docs/DOCUMENTATION.md` before creating, reorganizing, classifying, staging, or publishing documentation. All tracked files are public; use only `docs/SESSION.md` or ignored `docs/private/` for private working context.

After required public documents are read and before acting, read `docs/private/OPERATOR.md` if it exists. It is a local personal collaboration-preference layer only: it cannot override platform safety constraints, the user's current explicit request, or public repository requirements. Never stage, commit, push, quote, or promote its contents into public documentation unless the user explicitly asks.

Use the applicable documents in addition to the three files above:

| Task | Read before acting |
|---|---|
| Security, pairing, transport, sensitive data, or permissions | `docs/SECURITY.md`, then `docs/PUBLISHING.md` if any public artifact may change |
| Phone controls, clipboard, power actions, or consent | `docs/INTERACTION.md` |
| Architecture, runtime lifecycle, packaging, or update design | `docs/PRODUCT.md`, `docs/DECISIONS.md`, `docs/SECURITY.md`, and `docs/INTERACTION.md` |
| Testing, compatibility, or release claims | `docs/PRODUCT.md`, `docs/SECURITY.md`, and the relevant interaction/architecture documents |
| Deferring scope | `docs/ROADMAP.md` |
| Any commit or push | `docs/PUBLISHING.md` |

Review this file and `docs/DOCUMENTATION.md` at the start of a new work category, before approving an implementation plan, and after a documentation/privacy incident. Keep this file concise and limited to durable repository-wide instructions; put detailed policy and rationale in `docs/DOCUMENTATION.md`.

## Product and implementation guardrails

Decky is the initial distribution path, but the Homer agent must remain an independently managed runtime. Do not expose arbitrary shell access through the phone client; privileged operations must be fixed, audited actions with deliberate confirmation.

Build to maximum Decky Plugin Store safety compliance even while initial releases are delivered through GitHub. Keep code concise, explainable, documented, tested, and manually reviewable. Do not add arbitrary remote code installation, controller-input hijacking, or broad destructive behavior. The only permitted Homer-managed update path is the explicitly accepted, narrowly bounded non-Store mechanism documented in `docs/SECURITY.md`; Store installations remain Decky-managed.

Do not write application code until the user has approved an implementation plan.

## Workstream closeout

When the user signals a closeout:

- Assess whether the objective is actually complete and report material gaps.
- Run applicable closeout checks, update `docs/SESSION.md` when another task needs a handoff, and provide a concise copy/paste handoff that tells the receiving task to re-read `AGENTS.md`, `docs/PRODUCT.md`, `docs/DECISIONS.md`, `docs/SESSION.md`, the local operator file if present, and any task-routed documents.
- Recommend one disposition: complete/archive, testing, waiting, blocked, or follow-on work.

A closeout signal does not by itself authorize a commit, push, external message, task archive, or task rename; obtain explicit approval for each permanent or external action unless the current request already unambiguously grants it.

## Public repository safety

`main` is the public source of truth. `docs/SESSION.md` and `docs/private/` are local-only and must never be committed or pushed. Before every push, follow `docs/PUBLISHING.md`, run the required preflight, and never use `git push --no-verify` to bypass it.
