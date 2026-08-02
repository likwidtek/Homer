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

## D-006 — Single target machine in v0.1; multi-machine later

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** v0.1 manages one target machine. Multiple phones may pair, but only one may actively control that target at a time; the architecture must not preclude multi-machine support, which is required in a later release.
- **Rationale:** This keeps v0.1 focused on a reliable single-machine experience while accommodating a household and preserving a credible path to later multi-machine use and Wake-on-LAN relays.

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

- **Status:** Superseded by D-023
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

## D-022 — Public GitHub source and fail-closed publication preflight

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** GitHub repository `likwidtek/Homer` is Homer’s public source of truth. Keep `docs/SESSION.md` local-only and excluded from all public history. Every push must pass a versioned, fail-closed pre-push gate that checks repository hygiene, forbidden private-data paths, and secrets with Gitleaks; do not bypass it with `--no-verify`. Use GitHub secret scanning and push protection as independent backstops.
- **Rationale:** Vibe-coded projects need the same disciplined review and supply-chain protections as any other software. A local automated gate catches project-specific mistakes before publication, while GitHub provides a separate credential-detection layer.

## D-023 — Trusted-LAN HTTP with bearer authorization for v0.1

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer v0.1 will use HTTP and WebSockets on the local network, authorized by revocable per-phone bearer credentials after the approved QR pairing flow. This is explicitly a trusted-LAN model, not encrypted transport. Homer must warn users never to expose its port to the internet, including by router port forwarding or automatic router exposure, and must not intentionally create a public-internet listener.
- **Rationale:** This preserves a browser experience without certificate warnings or certificate-management setup. It deliberately accepts that HTTP cannot protect against a local-network attacker who can observe or alter traffic; the product must communicate that limitation accurately.

## D-024 — Explicit, short-lived local pairing mode

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer’s agent may remain available to already paired phones, but it must not continuously accept pairing. Only an explicit local action through Decky/controller interaction can enable pairing mode; it automatically expires after a short bounded window, rejects network-initiated activation, and requires local target approval before issuing a unique revocable phone credential.
- **Rationale:** This retains the intended Bluetooth-like, “stupid easy” user experience while reducing the opportunity for unnoticed or opportunistic pairing attempts.

## D-025 — v0.1 text keyboard and trackpad contract

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** v0.1 sends native phone-keyboard text and explicit pasted text to the machine as entered, with common non-native special keys and no synthesized held keys or automatic repeats. It provides a relative trackpad with left, middle, and right click, two-finger scrolling, and two-finger tap for right click. The intended supported experience is both Gaming Mode and Desktop Mode, in and out of games, subject to platform validation.
- **Rationale:** This delivers the practical couch-side keyboard/mouse replacement without expanding into complex gesture or gamepad emulation scope.

## D-026 — Text-only, consented clipboard retrieval

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Homer transfers text only. While an authorized remote browser is connected, the agent monitors the machine clipboard for convenient phone retrieval. The browser must disclose this and obtain a one-time persistent opt-in before displaying retrieved content. Homer keeps no clipboard history, logs, analytics, or background synchronization; consent remains until changed, phone revocation, or browser-data clearing.
- **Rationale:** The workflow is useful for troubleshooting and sharing text, while transparent consent and no retention prevent unexpected or secondary use of sensitive content.

## D-027 — Confirmed and cancellable power actions

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** Sleep, graceful shutdown, and restart require explicit phone confirmation. Restart and shutdown provide a short visible cancellable countdown. They remain fixed named actions, never arbitrary command execution.
- **Rationale:** The actions stay convenient from the couch without making accidental power changes too easy.

## D-028 — One target machine, multiple paired phones, exclusive control

- **Status:** Accepted
- **Date:** 2026-08-01
- **Decision:** v0.1 manages one target machine and permits multiple independently revocable paired phones. Only one phone may have an active control session at a time; a competing connection must be clearly rejected or explicitly handed off.
- **Rationale:** This accommodates a household without creating simultaneous-input conflicts or expanding v0.1 into multi-machine management.
