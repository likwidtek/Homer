# Product

## Status

Product design in progress. This document distinguishes confirmed intent from open questions. It is not an implementation specification yet.

## Product problem

Couch gamers using Steam-focused Linux machines often need to perform quick tasks—especially typing, using a mouse, handling text, or basic power actions—without getting up to connect a physical keyboard and mouse. This is particularly irritating on Steam Decks, Steam Machines, and DIY couch-gaming PCs.

## Vision

Create an open-source, privacy-first companion that lets a phone's web browser help control a paired couch-gaming machine on the local network. The experience should be simple, handy, and “stupid easy” for users who want to avoid physical peripherals.

## Intended users

- First user: the project owner.
- Early audience: technically inclined community users, including people reached through Reddit.
- Initial platform audience: people using Bazzite, SteamOS, Steam Deck, Steam Machines, or DIY Steam-machine-like Linux systems.

“Any OS” is a long-term aspiration, not an initial compatibility promise.

## Confirmed principles

- The phone experience is browser-only; a native iPhone or Android application is not the intended client.
- The product is open source.
- Homer is licensed GPL-3.0-or-later. Tips and donations are welcome and are separate from licensing.
- Privacy is a core tenet: local-network only, no cloud account, no cloud relay, no Homer-collected telemetry, and no remote access over the internet. Privacy-preserving aggregate repository and release statistics supplied by distribution platforms are permitted, but are not install or active-user metrics.
- Protect privacy and secrets, especially around pairing and clipboard content.
- Favor a simple, lazy-friendly, controller-friendly experience over broad feature scope.
- Support scalable distribution and portability: the same core should be able to reach Bazzite, SteamOS, other Linux systems, and potentially Windows later.
- Start with Decky Loader as the initial installation and distribution path, while keeping the local core independent of Decky at runtime.
- Treat maximum Decky Plugin Store safety compliance as a core project tenet even though the first public releases will be delivered through GitHub rather than the Store.
- Keep code concise, understandable, documented, tested, and manually reviewable. AI assistance does not remove responsibility for code quality or maintainability.
- Never expose a terminal or arbitrary command execution through the phone. Privileged behavior must be a small set of named, audited actions with deliberate confirmation.
- Decky recovery must be conservative by default: restore a usable Steam/Decky UI before offering more invasive repair or update actions.
- Homer’s independent agent aims to run rootlessly in normal operation; required permissions remain unvalidated. One-time setup or future recovery may still require narrowly scoped elevation. If future recovery requires elevated privileges, use a separate, optional, fixed-purpose local helper rather than a root network service.

## Confirmed v0.1 direction

### Primary job

Let a user use their phone browser as a quick keyboard and trackpad/mouse companion for a paired couch-gaming machine.

### Included functionality

- Keyboard functionality from the phone.
- Trackpad/mouse functionality from the phone.
- Text clipboard transfer with a goal of making machine-to-phone retrieval convenient for troubleshooting and sharing text elsewhere.
- Machine clipboard monitoring while the browser is connected, subject to later technical validation.
- Explicit phone-to-machine text transfer, such as pasting or typing into the web interface and sending it to the machine.
- Restart, graceful shutdown, and sleep actions.
- One actively paired and controlled machine. The architecture must not preclude later multi-machine support.
- A Decky plugin that installs/configures Homer, displays status and pairing information, and can be opened on demand for a quick connection.
- QR pairing that is designed to avoid static-IP setup and does not require a username or password.

### Installation and delivery direction

- Begin with Decky Loader as the initial installation and distribution path for Homer.
- Deliver the initial Decky plugin through GitHub releases. Do not make an official Decky Store listing a requirement for v0.1.
- Build toward Store compliance from the start: no self-updating agent, no arbitrary remote code installation, no controller-input hijacking, and no broad destructive behavior.
- Use the Decky plugin to bootstrap an independent, portable local Homer agent. The agent must run independently from Decky after installation.
- Keep the core adaptable for independent installation and other distribution methods later; these are required architectural considerations, not the initial delivery path.
- Plan an independent installation route for users without Decky and for recovery when Decky is unavailable; this follows the Decky-first initial delivery.
- Aim to make setup workable using a controller where feasible. For a machine that has no trusted software installed, zero machine interaction is not currently established as feasible.

### Future Decky recovery direction

Homer should be designed to become a recovery tool for Decky incompatibilities after it has been installed through a working Decky environment. The future recovery model is conservative by default:

1. Diagnose whether Decky’s loader is present and running.
2. Restore a usable Steam/Decky UI using low-risk actions, such as restarting Decky or temporarily disabling third-party plugins.
3. Only then offer separately confirmed, more invasive repair or update actions.

This is a confirmed strategic direction for v0.2. The exact recovery actions, required permissions, and validation matrix remain open.

## Pairing and secrets direction

- Pairing must be local-only and as easy as consumer Bluetooth-style pairing: no usernames, passwords, or static IP address setup.
- A QR code starts a short-lived, single-use pairing request; it must not itself be a reusable control credential.
- The target machine requires local approval through Decky/controller interaction before a phone receives a device-specific credential.
- Paired phones must be revocable.
- Future reconnection should use the paired device credential and local machine identity, not repeated QR scans or manually typed IP addresses.
- Local HTTP is preferred where it can meet the security requirement without friction. The exact transport and encryption design is unresolved; do not treat HTTP as permission to transmit secrets or clipboard data insecurely.

### Initial platform and validation direction

- SteamOS and Bazzite are required initial targets.
- Begin hands-on testing on the project owner’s Bazzite machine, then validate SteamOS directly before making v0.1 compatibility claims.

## Explicit v0.1 non-goals

- Remote view, screen sharing, or remote desktop; this is a later step.
- Force-quitting frozen games.
- Decky Loader repair, forced updates, or plugin repair.
- Steam, Flatpak, operating-system, Bazzite, or other maintenance updates.
- Installing or updating games from the phone.
- Windows support.
- A native mobile application.
- Multiple actively controlled machines.
- Wake-on-LAN; it is deferred pending research into a genuinely simple local delivery path.
- Internet-based remote access, cloud relays, accounts, or Homer-collected telemetry.
- Guaranteed automatic two-way clipboard synchronization.
- Arbitrary shell or terminal access from the phone.
- Decky/plugin recovery actions in v0.1.
- An official Decky Store listing in v0.1.

## Constraints and requirements

- The local machine needs trusted software capable of receiving input and performing approved actions; the phone browser alone cannot establish that on a new device.
- The product must not require Decky to keep functioning after setup.
- Decky-first installation does not by itself establish permission to distribute through the official Decky store. Store eligibility and policy compliance must be confirmed with Decky maintainers before it becomes a release dependency.
- GitHub-delivered Decky releases must still follow the Decky safety standards: versioned/reviewable agent artifacts, no self-update mechanism, no unverified executable downloads, no controller-input hijacking, and no code likely to broadly delete or corrupt data.
- Decky recovery actions must be explicitly allowlisted, locally auditable, and confirmed before execution. They must not download and execute arbitrary scripts.
- The QR pairing flow must require a short-lived, single-use bootstrap and local target approval before granting a revocable phone credential.
- Aim not to require root or sudo for the normal Homer agent at runtime. Required permissions for input and basic power actions must be validated on Bazzite and SteamOS before claiming rootless support. One-time setup may still require narrowly scoped elevation.
- A future elevated recovery helper, if needed, must not accept network requests directly and must expose only a fixed action allowlist.
- Phone-browser clipboard access is restricted by browser and operating-system permission behavior. The product must use explicit user actions where required and must not promise unsupported background synchronization.
- A sleeping machine cannot run its own agent. A phone browser cannot directly send a raw UDP Wake-on-LAN magic packet. Wake-on-LAN therefore needs an awake local relay/hub or another supported network path.
- Homer must not collect, transmit, or retain information about users, their machines, or their usage. Platform-provided aggregate repository traffic and release-download counts may inform project health, but must never be described as install or active-user counts.

## Product-level success criteria

- The project owner can test the primary keyboard/trackpad workflow on their Bazzite couch-gaming machine from a mobile browser.
- The workflow removes the need to reach for a physical keyboard/mouse for the supported quick tasks.
- Pairing and everyday use are simple enough to fit the project’s “lazy” and “stupid easy” intent.
- The design does not require cloud accounts, Homer-collected telemetry, or remote internet access.
- The architecture does not make Decky mandatory and leaves a credible route toward SteamOS and broader Linux support.
- If Decky becomes incompatible while the operating system, network, and Homer agent remain healthy, Homer’s v0.1 architecture preserves a planned v0.2 path to restore the Steam/Decky UI through conservative recovery actions.
- A new phone can pair through the controller-approved QR flow without entering a username, password, or static IP address.

## Assumptions and proposals (not confirmed requirements)

- The phone web interface may be a progressive web app or another browser-delivered interface.
- A Decky plugin may install, start, configure, or display pairing information for the independent agent.
- A future Decky recovery flow may restart Decky, temporarily disable third-party plugins, and use a versioned, verified repair procedure.
- The official Decky Store policy treats excessive generative-AI use as a potential rejection reason; Homer’s reviewability and specific Store eligibility must be confirmed before submission.
- An online paired machine may eventually relay Wake-on-LAN packets for another paired machine.
- A controller-only fallback installer may be possible through existing Steam/Desktop Mode tools.

## Open questions

1. What minimum viable Wake-on-LAN path can be genuinely simple for phone-browser users: an awake paired-machine relay, an optional separate hub, router integration, or another supported path?
2. What user experience and prerequisites should a later Wake-on-LAN feature require or explain?
3. What SteamOS validation plan, environment, and release criteria follow Bazzite-first hands-on testing?
4. What later multi-machine pairing and management experience should Homer support?
5. What clipboard content types, length limits, retention rules, refresh behavior, and sensitive-content safeguards are appropriate?
6. What exact first-run installation route will users without Decky follow, and what permissions will it require on Bazzite and SteamOS?
7. What transport and pairing approach satisfies the local-only privacy boundary while remaining simple in mobile browsers?
8. Is Homer eligible for distribution through the official Decky store when its plugin bootstraps an independent service? This must be confirmed with Decky maintainers; public terms/policy were not located during initial research.
9. Which exact Decky recovery actions are safe enough for v0.2, and which permissions do they require on Bazzite and SteamOS?
10. Can a frictionless local transport protect pairing secrets and clipboard data while retaining the desired browser experience, or is a trusted local HTTPS flow required?
11. What should the exact local-approval UX be when a new phone scans the QR code but the Steam UI is partially unhealthy?
12. What Bazzite and SteamOS permissions are required for rootless input injection, clipboard access, and basic power actions?
13. What versioned packaging and update path keeps the independent agent synchronized with GitHub-delivered Decky plugin releases without creating a prohibited self-updater?
