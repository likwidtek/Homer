# Testing and validation

## Status

Phase 0 architecture validation is approved. This document defines the evidence required before Homer’s final v0.1 architecture and implementation plan may be approved. It does not yet define the complete v0.1 release-test suite.

## Phase 0 objective

Determine whether the provisional Python/TypeScript architecture can satisfy Homer’s accepted packaging, lifecycle, platform, security, and browser requirements. Phase 0 produces evidence and decisions, not production components.

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

Using synthetic text and safe test targets, validate:

- rootless text and special-key injection in KDE, Steam Gaming Mode, in-game, and out-of-game fields;
- relative pointer motion, left/middle/right clicks, and scrolling without gamepad emulation or controller interception;
- connected-only clipboard observation and synthetic text setting, with monitoring stopped after disconnect;
- permission behavior for fixed sleep, restart, and shutdown requests without a root network service;
- service continuity across Steam-profile changes, Decky stopping, and Gaming/Desktop Mode transitions;
- binding only to explicitly eligible LAN addresses while excluding wildcard, public, global-IPv6, VPN, tunnel, container, and virtual-interface addresses; and
- stable random `.local` resolution plus visibly degraded direct-IP operation when mDNS is unavailable.

The candidate passes only for configurations where accepted functionality works within the documented rootless boundary. Any required one-time setup must be identified precisely and separately approved before it changes the machine.

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
- `.local` origin migration and degraded direct-IP behavior.

Required workflows must succeed on the physical Safari and Chrome targets without PWA installation, a service worker, CDN, cloud service, third-party executable asset, silent credential recovery, or simultaneous control leases.

## Evidence and result handling

Each test records an identifier, date, tester, sanitized OS/browser configuration, artifact version and hash, locked dependency manifest, reproduction steps, expected and observed result, and failure disposition. Results are classified as pass, pass with an explicitly accepted limitation, remediation required, or architecture rejected.

Detailed machine information, packet captures, and raw logs remain local. Public evidence must contain no device names, LAN addresses, credentials, clipboard content, pairing material, or other private data. Use only synthetic credentials and payloads.

## Probe-code and machine-safety boundary

Phase 0 permits disposable local harnesses only. It does not permit production Homer features, durable public APIs or schemas, real credentials or clipboard content, telemetry, automatic-update implementation, arbitrary command execution, permanent privileged helpers, internet exposure, port forwarding, or third-party runtime asset loading.

Keep probe code outside the repository unless it receives separate review and approval. Remove temporary services and state after testing. Obtain separate approval before installing packages, using elevation, changing firewall or network configuration, or otherwise mutating the test machine. Give a fresh explicit warning immediately before any real sleep, restart, or shutdown test.

## Architecture gate

Python remains the preferred agent technology if these tracks pass or have explicitly accepted narrow limitations. A material failure in packaging, reliability, platform capability, or reviewability triggers remediation or reconsideration of the agent language. A cryptographic implementation failure may justify a narrow native/WebAssembly module without rejecting the Python agent. Final architecture approval requires the combined Phase 0 evidence and unresolved-risk review.
