# Documentation standards

## Status

This is Homer’s public documentation governance policy. It defines what belongs in the repository, how documentation records decisions, and how local working context remains private. It complements `docs/PUBLISHING.md`; it does not replace its pre-push requirements.

## Principles

- Publish durable information a user, contributor, reviewer, or future maintainer needs to understand, evaluate, build, test, operate, or safely change Homer.
- Treat every public file as permanent public history. Write it so a stranger can understand it without private conversation context.
- Keep confirmed requirements, proposals, validation findings, and deferred work visibly distinct.
- Prefer updating an existing authoritative document over creating a duplicate or task-transcript file.
- Keep documentation concise, specific, dated where a decision record requires it, and reviewed with the same care as code.
- AI assistance does not reduce the required accuracy, safety review, or authorship accountability of documentation.

## Public and private boundary

All tracked documentation is public. It may contain product intent, security limitations, threat models, architecture, operational procedures, test plans, decision rationales, and concise repository/agent instructions. These are reviewable project artifacts, even when they help an AI agent.

Do not track personal working context, raw AI prompts or conversation transcripts, hidden reasoning, unreviewed research dumps, private to-do lists, device names, LAN addresses, screenshots, logs, captures, clipboard content, credentials, pairing material, certificates, customer information, or vulnerability details before coordinated disclosure.

`docs/SESSION.md` is the single local handoff record and is ignored. Other private documentation belongs under `docs/private/`, which is also ignored. `docs/private/OPERATOR.md` is the designated local collaboration-preference file. Private documents may shape personal workflow but cannot silently override public repository requirements. Neither may be staged, committed, or pushed. The pre-push gate rejects either path if it becomes tracked.

## Document map and authority

| Document | Authority and use | Create or update when |
|---|---|---|
| `README.md` | Public entry point and current high-level promise | A user-visible product, install, safety, or project-status statement changes |
| `docs/PRODUCT.md` | Confirmed product scope, constraints, non-goals, and open questions | Product scope or a product-level requirement changes |
| `docs/DECISIONS.md` | Consequential accepted, deferred, superseded, or dependency decisions | A decision is explicitly accepted, deferred, superseded, or made a release dependency |
| `docs/INTERACTION.md` | User-facing control, clipboard, and power behavior | The connected user experience or consent behavior changes |
| `docs/SECURITY.md` | Public threat model, security boundary, and security-release expectations | Security posture, sensitive-data handling, pairing, or exposure assumptions change |
| `docs/ROADMAP.md` | Explicitly deferred ideas; not commitments | Work is deferred beyond current scope or a previously deferred item changes status |
| `docs/PUBLISHING.md` | Public-repository and release hygiene | Publication workflow, review gates, or repository protection changes |
| `AGENTS.md` | Durable, repository-wide instructions for human and AI contributors | A cross-cutting workflow, safety rule, or required reading route changes |
| `docs/private/OPERATOR.md` | Local personal collaboration preferences; never a public project requirement | The project owner changes local collaboration preferences; never stage it |
| `docs/ARCHITECTURE.md` | Approved component boundaries, lifecycle, interfaces, and tradeoffs | Architecture is approved; do not create it for speculative designs |
| `docs/TESTING.md` | Approved test strategy, supported matrix, and release criteria | A test plan becomes stable enough to guide implementation or release claims |
| `docs/INSTALLATION.md` | Validated user installation and recovery instructions | An install/recovery route has been validated; never document aspirational steps as instructions |
| `CONTRIBUTING.md` and `SECURITY.md` reporting section | External contribution and vulnerability-reporting process | Before accepting external contributions or security reports |

Do not create a new top-level documentation file merely to preserve one meeting, prompt, exploration, or duplicate summary. Put temporary thinking in `docs/SESSION.md` or `docs/private/`; promote only the reviewed result to the relevant public document.

## Change protocol

For any product, architecture, security, or implementation change:

1. Identify the authoritative document(s) affected using the map above.
2. Record an explicit accepted/deferred/superseded decision in `docs/DECISIONS.md` when the change is consequential.
3. Update user-facing summaries (`README.md`) and detailed contracts when their statements would otherwise diverge.
4. Mark unvalidated ideas as proposals or open questions; do not silently turn them into requirements.
5. Update `docs/SESSION.md` at a natural handoff with current state, unresolved work, and the next safe step.
6. Before publication, follow `docs/PUBLISHING.md`; review documentation for private data as carefully as code.

## Workstream closeout

At a user-requested closeout, assess completion rather than assuming it. Report the scope outcome, deferred or blocked work, validation and review results, repository/upstream state, documentation coverage, and the exact permanent or external actions proposed. Update `docs/SESSION.md` and provide a compact copy/paste handoff when another task may continue the work. That handoff must instruct the receiving task to re-read `AGENTS.md`, `docs/PRODUCT.md`, `docs/DECISIONS.md`, `docs/SESSION.md`, the local operator file if present, and documents applicable under `AGENTS.md` task routing.

A closeout signal alone does not authorize a commit, push, external message, task archive, or task rename. Obtain explicit approval for each such action unless the current user request already unambiguously grants it. Recommend whether the task should be archived as complete, retained for testing or waiting, marked blocked, or continued as follow-on work.

## AI-oriented files

Repository instructions such as `AGENTS.md` are public when they contain durable project context and reviewable workflow rules. Keep them short, self-contained, and task-agnostic enough to help a new contributor. Store personal AI preferences in the user’s tool settings or private context; Homer’s local project-specific location is `docs/private/OPERATOR.md`.

Public AI-assisted artifacts must stand on their own for a human reviewer. They must not contain raw prompt history, model reasoning, private tool output, or claims that a model’s output was validated when it was not.

## Auditing this policy

Review this policy and `AGENTS.md` at the start of a new work category, when introducing a new documentation family or release process, before approving an implementation plan, and after any documentation/privacy incident. Keep `AGENTS.md` focused on actionable repository-wide rules; put longer rationale and classification detail here.
