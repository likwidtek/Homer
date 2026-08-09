# Testing and validation

## Status

Phase 0 architecture validation is approved. This document defines the evidence required before Homer’s final v0.1 architecture and implementation plan may be approved. It does not yet define the complete v0.1 release-test suite.

## Phase 0 objective

Determine whether the provisional Python/TypeScript architecture can satisfy Homer’s accepted packaging, lifecycle, platform, security, and browser requirements. Phase 0 produces evidence and decisions, not production components.

## Development and test-device change control

Use one private environment-change ledger on the primary development workstation as the source of truth for persistent changes made to any development or test device. This policy applies throughout development and testing, not only Phase 0. The authoritative ledger is `docs/private/ENVIRONMENT_CHANGES.md`; it is local-only and must never be staged, committed, or pushed. Remote devices do not maintain competing authoritative ledgers.

Log a change when it persists beyond the immediate operation, changes security or permissions, installs or removes tooling, starts or enables a service, changes an account or credential, alters network/firewall behavior, or could affect whether a result represents an ordinary user environment. Read-only inspection, no-op commands, and temporary files or processes removed within the same operation do not require entries unless an unexpected effect remains.

Before execution, record a compact entry containing:

- a stable change ID, device alias, workstream, and date;
- the intended change, reason, and approval status;
- whether it is temporary or an accepted baseline requirement;
- baseline impact as tooling-only, security-affecting, capability-affecting, or none;
- the action and verification method; and
- the rollback action and the event that should trigger it.

After execution, update only the observed result and status: `active`, `retained`, `rollback-due`, `rolled-back`, or `reconstructed/unknown`. One entry may cover a tightly related batch with the same purpose and rollback. Do not duplicate command output, test evidence, secrets, or raw logs in the ledger. Prefer device aliases over raw machine identifiers; include a local identifier only when safe rollback cannot be performed without it.

When work runs on a remote device and cannot update the central ledger directly, it must return a compact change receipt with the same fields through the existing secure management path. The coordinating task merges that receipt into the central ledger. Planned persistent changes should wait for that merge; urgent recovery work must be reconstructed immediately afterward.

At every device-related handoff and closeout, review active entries relevant to the work, identify changes whose purpose has ended, and either roll them back or obtain explicit approval to retain them. Compatibility and security conclusions must state whether they came from an ordinary baseline, a tooling-only deviation, or a capability/security-affecting deviation. Critical results influenced by a temporary deviation must be repeated after rollback or document that deviation as a required prerequisite.

## Track A — Python runtime and packaging

Evaluate a CPython `asyncio` agent using `aiohttp` and a PyInstaller one-folder artifact. Evaluate Nuitka only if PyInstaller materially fails a gate.

Validate that the artifact:

- needs no host Python or global package installation;
- runs on the tested Bazzite and current-stable SteamOS configurations;
- installs as a rootless graphical-session user service;
- survives Decky stopping, service restarts, Steam-profile changes, and Gaming/Desktop Mode transitions;
- runs for 24 hours and completes 50 clean service restarts;
- atomically activates between versioned directories and rolls back after interrupted activation or a failed health check;
- uninstalls without executable code, service definitions, credential-equivalent state, or active listeners; and
- can be built separately for Windows far enough to validate the packaging and platform-adapter boundary without making a Windows support claim.

The candidate passes when it becomes ready within three seconds in ten consecutive starts, has no failed start in 50 restarts, averages less than one percent idle CPU over ten minutes, uses no more than 100 MB idle resident memory, grows by no more than 10 MB without explanation over 24 hours, occupies no more than 150 MB installed and 75 MB compressed, and leaves a complete runnable old or new version after every simulated activation failure.

## Track B — Bazzite and SteamOS capabilities

### Host-access preflight

Before a probe that requires SSH or live graphical-session access, establish a local-console recovery path and a verified, non-default account credential where the platform requires one. Do not treat passwordless `sudo` as evidence that password-based SSH is configured or usable.

- On Bazzite, use the test account's locally managed password when SSH password authentication is temporarily needed. A deployment that permits passwordless elevation may still require the account password for SSH.
- On SteamOS/Steam Deck, set a unique password for the desktop account locally before attempting SSH; the default account is shipped without one.
- Prefer an authorized tester public key after the initial local setup. Verify key-based login in a second session before disabling SSH password authentication, and obtain separate approval before changing SSH, firewall, network, or account settings.
- Never record, transmit, paste into test artifacts, or reuse a real account password. Record only that the host-access preflight passed and the authentication method used.

Using synthetic text and safe test targets, validate:

- rootless text and special-key injection in KDE, Steam Gaming Mode, in-game, and out-of-game fields;
- relative pointer motion, left/middle/right clicks, and scrolling without gamepad emulation or controller interception;
- connected-only clipboard observation and synthetic text setting, with monitoring stopped after disconnect;
- stock graphical-user-service authorization for fixed sleep, restart, and shutdown requests, plus the proposed helper boundary where stock policy remains challenged;
- service continuity across Steam-profile changes, Decky stopping, and Gaming/Desktop Mode transitions;
- binding only to explicitly eligible LAN addresses while excluding wildcard, public, global-IPv6, VPN, tunnel, container, and virtual-interface addresses; and
- stable random `.local` resolution in each claimed mode, phone-side qualification before migration, prevention of migration after a known cross-mode failure, and visibly degraded direct-IP operation when mDNS is unavailable or unqualified.

The candidate passes only for configurations where accepted functionality works within the documented rootless-agent boundary. A power-helper path must demonstrate fixed local-only IPC, an exact action allowlist, no network listener, no blanket polkit rule, no arbitrary command path, deliberate installation, and clean removal. Any one-time setup or privileged probe must be identified precisely and separately approved before it changes the machine.

## Track C — encrypted-channel selection

Compare a narrowly wrapped, pinned Noise implementation suitable for native and WebAssembly builds with other maintained candidates that meet the same assurance bar. Analyze the proposed pairing and reconnection patterns before selecting them; `NNpsk0` is a candidate, not an accepted pattern.

Validation must:

- pin the protocol revision, complete protocol name, primitives, prologue, and Homer protocol version;
- establish the required authentication, freshness, forward-secrecy, channel-binding, and replay properties;
- pass every applicable official vector;
- prove native/browser interoperability and interoperability with at least one independent implementation;
- reject replayed, reordered, duplicate, altered, truncated, oversized, wrong-version, wrong-state, and invalid-secret messages;
- fuzz the exposed parser/state boundary without crashes, panics, deadlocks, permanent resource consumption, or sensitive diagnostic output;
- enforce the ten-second unauthenticated WebSocket deadline;
- load all executable browser assets locally; and
- complete dependency, license, maintenance, unsafe-code, vulnerability, and manual review.

The candidate fails for any action performed after an authentication or integrity failure, any known unaddressed high- or critical-severity vulnerability, incompatible licensing, or an implementation too opaque or broad for meaningful review. A missing formal implementation audit is residual risk requiring explicit review; it is not silently treated as assurance.

## Track D — physical browser validation

Required devices are a current iPhone using Safari and a current Android device using Chrome. Evaluate Firefox on Android without promising support until it passes. Use a trusted home LAN with working mDNS and a controlled trusted-LAN condition where mDNS is unavailable.

Validate:

- complete local-HTTP loading and local JavaScript/WebAssembly cryptography;
- encrypted WebSocket establishment and recovery;
- IndexedDB credential persistence through asset-cache clearing and loss after site-data clearing;
- phone background/foreground, lock/unlock, Wi-Fi interruption, and same-phone reconnection behavior;
- native keyboard composition, correction, paste, special keys, touch tracking, scrolling, and two-finger tap without browser gesture interference;
- explicit phone-to-machine paste and a clear selectable-text fallback when programmatic phone clipboard copy is unavailable; and
- qualified `.local` origin migration across claimed modes, refusal to migrate after a known relevant failure, non-duplication of durable credentials across origins, and degraded direct-IP behavior.

Required workflows must succeed on the physical Safari and Chrome targets without PWA installation, a service worker, CDN, cloud service, third-party executable asset, silent credential recovery, or simultaneous control leases.

## Evidence and result handling

Each test records an identifier, date, tester, sanitized OS/browser configuration, artifact version and hash, locked dependency manifest, reproduction steps, expected and observed result, and failure disposition. Results are classified as pass, pass with an explicitly accepted limitation, remediation required, or architecture rejected.

Detailed machine information, packet captures, and raw logs remain local. Public evidence must contain no device names, LAN addresses, credentials, clipboard content, pairing material, or other private data. Use only synthetic credentials and payloads.

## Probe-code and machine-safety boundary

Phase 0 permits disposable local harnesses only. It does not permit production Homer features, durable public APIs or schemas, real credentials or clipboard content, telemetry, automatic-update implementation, arbitrary command execution, permanent privileged helpers, internet exposure, port forwarding, or third-party runtime asset loading.

Keep probe code outside the repository unless it receives separate review and approval. Remove temporary services and state after testing. Obtain separate approval before installing packages, using elevation, changing firewall or network configuration, or otherwise mutating the test machine. Give a fresh explicit warning immediately before any real sleep, restart, or shutdown test.

## Architecture gate

Python remains the preferred agent technology if these tracks pass or have explicitly accepted narrow limitations. A material failure in packaging, reliability, platform capability, or reviewability triggers remediation or reconsideration of the agent language. A cryptographic implementation failure may justify a narrow native/WebAssembly module without rejecting the Python agent. Final architecture approval requires the combined Phase 0 evidence and unresolved-risk review.
