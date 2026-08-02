# Decisions

## Status

These records capture consequential decisions explicitly stated or approved in product discussion. They do not settle open implementation details.

## D-001 — Browser-only phone client

- **Status:** Accepted
- **Date:** 2026-07-29
- **Decision:** The phone-facing client will be a web/browser experience rather than a required native iPhone or Android application.
- **Rationale:** The desired experience should load on mobile web browsers and avoid the irritation of maintaining a native mobile app.

## D-002 — Start with phone keyboard and trackpad/mouse

- **Status:** Accepted
- **Date:** 2026-07-29
- **Decision:** The initial primary product job is a phone keyboard and trackpad/mouse for a couch-gaming machine.
- **Rationale:** This directly addresses the frequent need to connect a physical keyboard or mouse for quick tasks.

## D-003 — Local-only privacy boundary

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** The product is strictly local-network only: no cloud accounts, cloud relays, Homer-collected telemetry, or remote access over the internet. Privacy-preserving aggregate repository and release statistics supplied by distribution platforms are permitted, but are not install or active-user metrics.
- **Rationale:** Privacy and protection of secrets are core product principles.

## D-004 — Independent agent runtime

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Build a portable independent local agent as the core runtime. It must continue operating independently after its installation, even if Decky Loader later fails.
- **Rationale:** Decky is valuable for a plug-and-play experience but may be unavailable or broken; the companion must remain reachable as a recovery path.

## D-005 — Initial platform direction

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** SteamOS and Bazzite are required initial targets. Begin hands-on testing on the project owner’s Bazzite machine, then validate SteamOS directly before making v0.1 compatibility claims.
- **Rationale:** Bazzite is immediately available for testing; SteamOS is an intended initial target.

## D-006 — Single-machine v0.1; multi-machine later

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** v0.1 supports one actively paired and controlled machine. Its architecture must not preclude multi-machine support, which is required in a later release.
- **Rationale:** This keeps v0.1 focused on a reliable single-machine experience while preserving a credible path to later multi-machine use and Wake-on-LAN relays.

## D-007 — v0.1 power actions

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Include restart, graceful shutdown, and sleep in v0.1.
- **Rationale:** These are basic couch-side actions within the initial companion direction.

## D-008 — Wake-on-LAN deferred pending ease-of-use research

- **Status:** Deferred
- **Date:** 2026-08-01
- **Decision:** Wake-on-LAN is not a v0.1 capability. Research the minimum viable path that is genuinely simple for phone-browser users, including available local relays, hubs, and router integrations, before assigning it to a later release.
- **Rationale:** A phone browser cannot directly perform raw UDP broadcast and a sleeping target cannot host its agent. Requiring unexplained extra hardware or complex network configuration would conflict with Homer’s ease-of-use goal.

## D-009 — Clipboard direction

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Aim for convenient machine-to-phone clipboard retrieval through machine clipboard monitoring while connected, and use explicit phone-to-machine transfer. Do not promise guaranteed automatic two-way synchronization.
- **Rationale:** The user wants clipboard sharing to be as close to bidirectional synchronization as mobile-browser limits allow.

## D-010 — Screen share is deferred

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Remote view/screen share is a later step, not v0.1.
- **Rationale:** It is valuable but is a separate scope and must not delay the initial input companion.

## D-011 — Decky-first initial delivery

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Start the project with Decky Loader as Homer’s initial installation and distribution path. The Decky plugin bootstraps the independent agent; alternative installers will follow.
- **Rationale:** Existing Decky users are a likely early audience, and a controller-friendly Decky path best serves the project’s “lazy” and “stupid easy” goal.

## D-012 — Conservative Decky recovery and fixed privileged actions

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Design Homer to support future Decky recovery. Recovery defaults to restoring a usable UI first, then offers separately confirmed repair or update actions. Homer must never expose arbitrary shell or terminal access through the phone; privileged actions are fixed, audited, and confirmation-gated.
- **Rationale:** Decky/plugin incompatibility is an important pain point and a potential differentiator, but unrestricted phone-triggered command execution would be unsafe.

## D-013 — Official Decky store eligibility remains unconfirmed

- **Status:** Open dependency
- **Date:** 2026-08-01
- **Decision:** Do not assume that the official Decky store will accept a plugin that bootstraps an independent Homer service. Seek confirmation from Decky maintainers before making store availability a release dependency.
- **Rationale:** Decky publishes safety rules for Store submissions, but they do not establish whether a plugin that bootstraps Homer’s independent agent will be accepted. Confirm that architecture with maintainers before relying on Store availability.

## D-014 — GPL-3.0-or-later licensing

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer will use GPL-3.0-or-later. Tips and donations are welcome and separate from licensing.
- **Rationale:** Homer should be genuinely open source and community-friendly while requiring distributed improvements to remain open.
- **Deferred before public release or outside pull requests:** trademark/brand policy, formal trademark consideration, name clearance, and `CONTRIBUTING.md` with DCO sign-off.

## D-015 — v0.2 conservative Decky recovery

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Actual Decky and plugin recovery features belong in v0.2. v0.1 must establish the independent agent, installer, and lifecycle foundations required for them.
- **Rationale:** Recovery is a high-value differentiator, but should not turn the first release into an unvalidated privileged repair-tool project.

## D-016 — Rootless normal-operation target; optional isolated elevation

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer’s independent agent aims to run rootlessly in normal operation; required permissions remain unvalidated. One-time setup or future v0.2 recovery may require narrowly scoped elevation. If recovery needs elevated access, isolate it in an optional, fixed-purpose local helper with no network listener.
- **Rationale:** This minimizes the security impact of a phone-controlled local service while preserving a route for future Decky repair.

## D-017 — QR pairing with local approval

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Pair new phones through a short-lived, single-use QR bootstrap followed by local approval on the target through Decky/controller interaction. Store a unique revocable credential per paired phone; do not use usernames, passwords, reusable QR tokens, or static-IP setup.
- **Rationale:** Pairing must be extremely simple while preventing a nearby person from silently taking control by scanning or photographing a QR code.

## D-018 — Local HTTP preference, secure transport unresolved

- **Status:** Accepted direction; implementation unresolved
- **Date:** 2026-08-01
- **Decision:** Prefer local HTTP where it can satisfy the security and ease-of-use goals. Do not expose secrets or clipboard data insecurely merely to avoid browser friction.
- **Rationale:** The desired experience is local and frictionless, but pairing and clipboard require strong protection.

## D-019 — Decky Store safety compliance, GitHub-first distribution

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Build Homer for maximum Decky Plugin Store safety compliance from the outset, but distribute the initial Decky plugin through GitHub releases. An official Store listing is a later goal, not a v0.1 dependency.
- **Rationale:** The Store’s review and safety standards align with Homer’s security goals, but Store acceptance is uncertain and must not block delivery.
- **Required design implications:** no self-updating agent, arbitrary remote code installation, controller-input hijacking, or broad destructive behavior. Keep code concise, explainable, documented, tested, and manually reviewable; AI assistance does not waive these responsibilities.

## D-020 — No Homer-collected user telemetry

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer will not collect, transmit, or retain information about users, their machines, or their usage. Project health may use privacy-preserving aggregate statistics already provided by distribution platforms, such as GitHub repository traffic and release-asset download counts; these must not be represented as install or active-user counts.
- **Rationale:** This preserves the local-only, privacy-first model while allowing limited awareness of public distribution reach without adding tracking code or a Homer-operated analytics service.

## D-021 — Security design principles

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Apply least privilege, fixed allowlisted actions, local-only operation, explicit local opt-ins for any widened capability, and narrowly bounded access to sensitive local data. Homer’s transport, pairing, privilege, and update designs remain subject to Homer’s own requirements and validation.
- **Rationale:** These principles align with Homer’s privacy-first, browser-only, no-self-updater direction while keeping security-sensitive implementation details open until they are validated.
