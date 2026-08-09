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
- **Required design implications:** Store installations use Decky’s update path. The only non-Store exception is the bounded signed-update mechanism accepted in D-033; arbitrary remote code installation, controller-input hijacking, and broad destructive behavior remain prohibited. Keep code concise, explainable, documented, tested, and manually reviewable; AI assistance does not waive these responsibilities.

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

- **Status:** Superseded by D-039 and D-042
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
- **Decision:** Homer transfers text only. While an authorized remote browser is connected, the agent monitors the machine clipboard for convenient phone retrieval. The browser must disclose this and obtain a one-time persistent opt-in before displaying retrieved content. Homer keeps no clipboard history, logs, analytics, or background synchronization; consent remains until changed, phone revocation, or browser site-data clearing.
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

## D-029 — Public documentation governance and private working context

- **Status:** Accepted
- **Date:** 2026-08-02
- **Decision:** All tracked repository documentation is public, durable, and reviewable. `docs/DOCUMENTATION.md` defines its authority, lifecycle, and when to create a new document. `docs/SESSION.md` and ignored `docs/private/` are the only designated local documentation areas; they must never be staged, committed, or pushed. `AGENTS.md` must route task types to their governing documentation and be audited at new work-category and plan-approval boundaries.
- **Rationale:** Open source benefits from transparent, maintainable project context, while private working notes, sensitive material, and raw AI artifacts must stay out of public history.

## D-030 — Local operator-preference instruction layer

- **Status:** Accepted
- **Date:** 2026-08-02
- **Decision:** When present, ignored `docs/private/OPERATOR.md` is read after required public documentation and before work begins. It contains only the project owner’s private collaboration preferences. It may not override platform safety constraints, current explicit user instructions, or public repository requirements, and its contents must never be staged, committed, pushed, quoted, or promoted into public documentation without explicit user direction.
- **Rationale:** This gives the project owner a durable private communication layer without making hidden preferences an unreviewable source of public product or engineering policy.

## D-031 — Explicit-approval workstream closeout

- **Status:** Accepted
- **Date:** 2026-08-02
- **Decision:** A user signal to close a workstream requires a completion assessment and a structured closeout report, including validation, documentation coverage, repository state, remaining work, and a copy/paste handoff. It does not by itself authorize a commit, push, external message, task archive, or task rename; each permanent or external action requires explicit approval unless the current request already unambiguously grants it. A closeout recommends one disposition: complete/archive, testing, waiting, blocked, or follow-on work.
- **Rationale:** This creates a repeatable, reviewable endpoint for every task while preserving the owner’s deliberate control over public changes and task lifecycle.

## D-032 — Handoff requires fresh task-context reading

- **Status:** Accepted
- **Date:** 2026-08-02
- **Decision:** Every closeout handoff must tell a receiving new or resumed task to re-read `AGENTS.md`, `docs/PRODUCT.md`, `docs/DECISIONS.md`, `docs/SESSION.md`, the local operator-preference file if present, and the documents required by task routing before acting.
- **Rationale:** A parent or newly created task may not contain the latest decisions or local handoff state. Explicit fresh reading prevents stale context from silently governing the next workstream.

## D-033 — Channel-owned, bounded Homer updates

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** The installation channel owns Homer updates. Official Decky Plugin Store installations use Decky’s update path exclusively and show the installed version only. GitHub-sideloaded Decky and standalone installations may manually check for and install updates from Homer’s fixed official source. Their first manual check may invite, but must not require, opt-in to automatic checks or automatic checks and installation; the user may choose a daily or weekly schedule. Any paired/authorized phone or the local machine may request a manual check or installation. Automatic installation waits for no active remote control session. Updates must use signed metadata and artifacts, hash verification, staged atomic activation, a health check, and automatic rollback. Homer must not update Decky Loader or its plugin files through this path.
- **Rationale:** This gives non-Store users a set-it-and-forget-it option without making every installation a remote updater, while respecting Decky’s ownership and review model for Store installations. The bounded verification and rollback rules keep the convenience path narrow, auditable, and recoverable.

## D-034 — User-session independent-agent lifecycle

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Homer’s independent agent runs as the normal couch-gaming Linux user’s background service and starts with that user’s graphical session. It remains available across Steam Gaming Mode, Desktop Mode, and Steam-profile changes. It is unavailable before that Linux user logs in after boot. Decky failure, disablement, or Steam UI trouble must not stop an already-running agent. An intentional Homer uninstall stops the agent and removes its local state.
- **Rationale:** This keeps the network-facing runtime rootless and tied to the user who owns the desktop/input session, avoids duplicate services for shared Steam profiles, and preserves Homer’s independent recovery value without introducing a pre-login privileged service.

## D-035 — v0.1 component boundaries and Decky-unavailable pairing

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** The independent agent owns the local HTTP/WebSocket service, paired-device credentials, exclusive control-session state, fixed input/clipboard/power requests, non-Store update verification and rollback, and persistent Homer state. The Decky plugin owns installation/bootstrap, service status, device management, and local pairing-mode enablement and approval. The phone browser owns the authenticated control and consent experience only. A local machine interface may manage installation, status, manual update checks, and uninstall, but is neither a remote-control surface nor a second phone-facing API. If Decky is unavailable, already paired phones remain usable but new pairing is unavailable in v0.1.
- **Rationale:** This keeps sensitive machine authority in the independent local agent, makes the browser and Decky layers narrow and reviewable, and avoids weakening pairing with an unvalidated fallback approval path while retaining usefulness during a Decky failure.

## D-036 — Separate Store and non-Store update artifacts

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Official Decky Plugin Store, GitHub-sideloaded Decky, and standalone installations use separate distribution artifacts, not a writable channel marker. The Store artifact excludes Homer-managed update code and interfaces, including checking, downloading, installation, release-source configuration, automatic scheduling, and phone/CLI update routes. The sideloaded-Decky and standalone artifacts may contain only D-033’s bounded updater. Store build/review evidence must demonstrate that update capability is absent from its artifact. Homer does not attempt to prevent a machine owner from deliberately replacing their installed software, but no paired phone, network request, or editable configuration may convert a Store artifact into a self-updating artifact.
- **Rationale:** An editable marker would not be a meaningful trust boundary. Separate artifacts give users and Store reviewers an auditable, technical guarantee that an official Store build cannot quietly acquire a Homer-managed update path.

## D-037 — v0.1 delivery channels

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Homer v0.1 is delivered Decky-first through GitHub releases, with a signed portable user-space standalone bundle as the no-Decky and recovery route. `ujust`, Bazzite Portal, Bazaar/Flatpak, Homebrew, and RPM layering are not Homer v0.1 delivery channels. Any later package-manager channel requires its own explicit update-ownership and compatibility decision.
- **Rationale:** Decky is the lowest-friction initial path. A portable user-space bundle preserves independent operation without tying Homer to a particular Linux package manager or weakening its host-integration requirements. Avoiding Bazzite-specific and system-layered installation methods keeps v0.1 supportable and consistent with least privilege.

## D-038 — Narrow initial compatibility claim

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Initial compatibility claims are limited to the tested Bazzite KDE Desktop Mode and Steam Gaming Mode configuration, and the tested current-stable Steam Deck/SteamOS configuration. Homer makes no v0.1 claim for Bazzite GNOME, NVIDIA-specific behavior, other desktop environments, other hardware, or untested SteamOS variants.
- **Rationale:** Bazzite and SteamOS vary materially by desktop environment, compositor, hardware, and configuration. A narrow claim gives the project a reliable Bazzite-first start and prevents unvalidated support promises.

## D-039 — Authenticated application-layer protection for remote messages

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Homer continues to serve its browser interface and upgrade connections over local HTTP/WebSockets, but v0.1 must protect every keyboard, mouse, clipboard, status, power, and remote-management message inside an authenticated application-layer encrypted channel. Pairing and reconnection use 256-bit pre-shared secrets with an established, independently reviewed protocol family such as Noise; Homer must not invent its own cryptographic handshake. The exact protocol pattern and pinned browser/agent implementation are implementation-plan dependencies and must pass official vectors, cross-implementation tests, dependency review, and browser/platform validation before release.
- **Rationale:** This meaningfully protects sensitive traffic from passive LAN observation and unauthenticated message injection while retaining certificate-free browser onboarding. It is not HTTPS-equivalent: an active LAN attacker can alter the HTTP-delivered browser code and defeat the protection, so Homer’s trusted-LAN boundary remains essential.

## D-040 — Exact v0.1 pairing bootstrap and approval contract

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** A local Decky/controller action enables one pairing window for five minutes and creates a 256-bit single-use bootstrap secret. The QR carries that secret in its URL fragment; the browser must remove the fragment from visible URL/history state immediately after reading it, and the secret is never sent as plaintext. A successful pre-shared-key handshake binds one pending request and consumes the bootstrap for further attempts. The phone and Decky display the same six-digit code bound to that handshake; local approval or rejection must occur within two minutes. Approval delivers a unique 256-bit per-phone secret only through the encrypted pairing channel. No LAN request can create, extend, or reopen pairing mode.
- **Rationale:** The flow remains scan-and-approve simple while limiting replay, races, accidental approval, URL leakage, and unattended pairing exposure.

## D-041 — Hybrid discovery, canonical reconnection, and bounded listeners

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** The pairing QR initially uses the agent’s current eligible RFC1918 IPv4 address so first contact does not depend on multicast DNS. During pairing, the browser automatically tests a stable, randomly generated `.local` hostname. When it works, a separate short-lived single-use encrypted migration claim moves durable browser state to that canonical origin for reconnection across DHCP changes and removes the credential from the temporary IP origin. When it does not, Homer remains usable through the current address but identifies discovery as degraded and explains that an address change may require locally approved reconnection. The agent answers its high-entropy direct mDNS hostname without advertising an enumerable Homer service. It binds only to explicitly selected eligible RFC1918 LAN addresses, never wildcard, public, global-IPv6, VPN/tunnel, or container/virtual addresses; it creates no UPnP or NAT-PMP mapping. IPv6-only and segmented/isolated networks are outside the v0.1 compatibility claim.
- **Rationale:** Direct-IP bootstrap tolerates multicast-hostile home equipment, while the canonical `.local` origin removes ordinary DHCP friction. Narrow address selection and no automatic router mapping reduce accidental exposure without pretending software can defeat deliberate port forwarding or hostile network infrastructure.

## D-042 — Per-phone secret lifecycle and exclusive control lease

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Each paired browser stores its 256-bit phone secret in origin-scoped IndexedDB; the agent stores the corresponding credential-equivalent secret material in owner-only state. The secret is never placed in ordinary requests, URLs, logs, diagnostics, or telemetry and does not rotate on a timer in v0.1. It remains valid until locally revoked, self-revoked through an authenticated channel, or Homer is uninstalled. Revocation immediately terminates that phone’s sessions. Each WebSocket must complete its authenticated encryption handshake within ten seconds. An authenticated phone explicitly acquires a thirty-second renewable control lease; the same phone may replace its stale prior connection immediately, while another phone is clearly denied control until release or expiry. v0.1 has no remote kick-over or cross-phone handoff.
- **Rationale:** Stable credentials preserve the “stupid easy” return experience, per-phone revocation bounds access, and a short renewable lease resolves abandoned mobile connections without allowing simultaneous input.

## D-043 — Trusted-LAN disclosure and browser-data recovery boundary

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Decky first run and each new-phone pairing must plainly disclose once that Homer is for a trusted home network, sensitive messages are protected only after pairing, the browser interface itself arrives over HTTP, an active hostile network can defeat that protection, and Homer must never be port-forwarded or used on public/shared Wi-Fi. The user explicitly continues past the disclosure; normal UI retains a compact trusted-LAN status link. Ordinary browser asset-cache clearing must not unpair a phone because credentials live in IndexedDB. Clearing or losing origin site data removes the phone’s only credential copy and requires locally approved re-pairing; Homer must not use device fingerprinting, URL-carried recovery credentials, or misleading Keychain/Keystore claims. The old agent-side phone record remains revocable until the owner removes it.
- **Rationale:** Users receive an accurate, comprehensible promise without repeated warning fatigue, and recovery remains secure rather than silently replacing authentication with fingerprinting or exposed secrets.

## D-044 — Python-first, validation-gated agent technology

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Python is the preferred technology for Homer’s independent agent, with TypeScript for the browser and Decky user interfaces. This is a validation-gated direction rather than final stack approval. The agent must first pass packaging, rootless lifecycle, resource-use, update/rollback, Bazzite, SteamOS, and future-platform-boundary probes. Browser cryptography may use a narrowly scoped, locally bundled JavaScript or WebAssembly module and does not determine the language of the rest of the agent. Rust remains a fallback if Python fails a material validation gate or may be confined to a reviewed cryptographic module when the browser/agent interoperability review justifies it. No production dependency or exact protocol implementation is accepted by this decision.
- **Rationale:** Python aligns with Decky’s conventional backend model, keeps the main project accessible and maintainable, and is sufficient for Homer’s event-driven workload. Separating the cryptographic implementation from the agent-language choice avoids imposing a broader Rust architecture solely to satisfy browser cryptography, while explicit gates preserve packaging, security, portability, and future Windows options.

## D-045 — Approved Phase 0 architecture-validation probes

- **Status:** Accepted
- **Date:** 2026-08-03
- **Decision:** Before final architecture or application-code approval, Homer will run the bounded Phase 0 plan in `docs/TESTING.md`. It validates the provisional Python agent’s packaging, resource use, user-session lifecycle, atomic activation and rollback; rootless Bazzite and SteamOS input, clipboard, power, network-binding, mDNS, and Decky-independence capabilities; the exact pre-shared-key protocol pattern and native/browser implementations; and physical iOS Safari and Android Chrome behavior. Phase 0 permits only disposable local probes within that document’s safety and evidence limits. It does not authorize production Homer features, durable APIs, real credentials or clipboard data, arbitrary commands, permanent privileged helpers, update implementation, public exposure, or unreviewed repository publication of probe code.
- **Rationale:** The unresolved risks are empirical platform, packaging, and implementation questions. A narrow evidence-producing phase breaks the circular dependency between final technology selection and implementation-plan approval without allowing exploratory code to become an unreviewed product foundation.

## D-046 — Central development and test-device change ledger

- **Status:** Accepted
- **Date:** 2026-08-04
- **Decision:** Persistent, security-relevant, or result-affecting changes made to development and test devices must be recorded in one private environment-change ledger on the primary development workstation. Each entry identifies why the change exists, whether it departs from an ordinary user environment, how it is verified, when it should be reconsidered, and how it is rolled back. Remote tasks return compact change receipts to the coordinating task instead of maintaining competing authoritative ledgers. Device-related handoffs and closeouts reconcile active changes and prompt for rollback or explicit retention. Read-only and self-cleaning ephemeral work is excluded.
- **Rationale:** Development and compatibility work can otherwise accumulate forgotten services, permissions, tooling, and configuration that weaken security or produce misleading results. A single concise ledger preserves rollback accountability and baseline clarity without adding an external system or logging every command.

## D-047 — Approval-efficient autonomous work

- **Status:** Accepted
- **Date:** 2026-08-08
- **Decision:** Approval of a bounded plan or a request to change, build, fix, or execute authorizes Codex to complete all safe in-scope intermediate work without repeated confirmation. Safe work includes read-only inspection and research, in-scope workspace edits, non-destructive validation, and approved self-cleaning ephemeral probes. Codex should continue independent safe work around a blocked step and batch foreseeable approval needs. Fresh approval remains required for commits and pushes, external writes, destructive or difficult-to-recover actions, purchases, material scope expansion, unauthorized secret use, elevation, real power actions, and persistent, security-relevant, or result-affecting development/test-device changes.
- **Rationale:** Explicitly distinguishing ordinary execution from consequential approval boundaries reduces idle turns and ceremonial pauses without weakening repository publication, device-baseline, privilege, security, or physical-safety controls.

## D-048 — Conditional fixed-purpose local power helper

- **Status:** Accepted
- **Date:** 2026-08-08
- **Decision:** Homer first uses the operating system’s normal logind power interfaces when the stock policy authorizes the graphical-session user agent. If supported Bazzite or SteamOS configurations still require interactive authorization, Homer may offer a deliberate one-time local installation of an optional, removable, fixed-purpose privileged power helper. The helper has no network listener, accepts requests only through narrow local IPC from the normal-user agent, exposes only sleep, restart, shutdown, and required cancellation, and provides no shell or arbitrary-command path. Homer must not install a blanket polkit rule. If neither the stock interface nor the optional helper is available, the phone UI clearly disables or hides unavailable power actions rather than presenting a broken control.
- **Rationale:** Phone confirmation alone cannot satisfy an operating-system authorization challenge, while per-use machine authentication defeats the couch-side workflow. A separate local allowlisted helper preserves a rootless network-facing agent and confines elevation more narrowly than weakening power authorization for every process owned by the desktop user.

## D-049 — Qualified `.local` migration with direct-IP degraded operation

- **Status:** Accepted
- **Date:** 2026-08-08
- **Decision:** Direct private IPv4 remains Homer’s first-contact path and a first-class visibly degraded operating mode. Homer migrates a phone credential to the stable random `.local` origin only when the supported platform/configuration is eligible for canonical migration and that phone validates the hostname in the relevant environment; a single Desktop Mode success must not override a known cross-mode failure. Homer does not retain duplicate durable credentials at both IP and `.local` origins. Configurations that fail qualification remain on the IP origin and warn that an address change can require local reconnection. Silent recovery or automatic requalification after a later mode transition is not required in v0.1.
- **Rationale:** Origin-scoped browser credentials make an unreliable canonical hostname worse than a disclosed IP fallback, while retaining credentials at a recycled IP origin creates avoidable exposure. Qualification preserves convenient DHCP-resistant reconnection where it is reliable without stranding users on tested multicast-hostile configurations or weakening the accepted credential boundary.
