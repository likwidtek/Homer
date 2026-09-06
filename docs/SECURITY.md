# Security and connection model

## Status

This document preserves Homer’s accepted v0.1 security and connection intent. It is a product-security model, not an implementation protocol or a claim of formal security review. Implementation choices must conform to the accepted decisions in `docs/DECISIONS.md` and be validated before release.

## User promise and boundary

Homer is a local-network companion for a trusted home LAN. It has no cloud account, relay, or telemetry service, and it does not provide remote internet access. Its sole intended outbound-internet exception is checking for or fetching a verified Homer update for a non-Store installation, as described below. An official Decky Plugin Store installation has no Homer-managed update traffic; Decky owns that update path.

Homer v0.1 uses HTTP and WebSockets rather than HTTPS/WSS to avoid certificate warnings and certificate-management setup in mobile browsers. After pairing, Homer protects sensitive remote messages inside an authenticated application-layer encrypted channel. This protects the genuine Homer client’s keyboard, mouse, clipboard, status, power, and management messages from passive LAN observation and unauthenticated injection.

This protection is not HTTPS-equivalent. The browser interface itself arrives over HTTP, so an active LAN attacker can replace that code, steal pairing material, or defeat its message protection. Traffic timing, connection metadata, and the public browser assets are not confidential. Users must never port-forward Homer, expose it through a reverse proxy, enable automatic router exposure for it, or use it on an untrusted/shared network.

Homer must state this boundary plainly during first-run setup and in user-facing documentation. It must not intentionally create a public-internet listener or automatic router mapping.

## Non-Store update boundary

GitHub-sideloaded Decky and standalone Homer installations may check Homer’s fixed official release source for an update. A manual check is always available to an authorized paired phone or the local machine. On its first use, Homer may invite the user to enable automatic updates, but the prompt must not block the requested manual check.

Automatic behavior is opt-in and user-selectable: automatic checks, or automatic checks and installation, on a daily or weekly schedule. The default is manual-only. Automatic installation must wait until no remote control session is active. An authorized paired phone or the local machine may also explicitly request installation.

The updater is not a general downloader or remote-command feature. It must use only the fixed official source and release formats defined by Homer, authenticate release metadata and artifacts with a release-signing public key shipped with Homer, verify the declared artifact hash before activation, stage and validate a new version before atomically activating it, and automatically roll back to the last known-good version if the post-update health check fails. It must not accept a phone-supplied URL, executable, command, or arbitrary version path.

The update UI must disclose that the release source receives ordinary request metadata under that source’s privacy practices. Homer must not add its own device identifier, usage payload, installation-success ping, or heartbeat. Platform-provided aggregate release-download statistics remain downloads, not confirmed installations or successful updates.

Store, sideloaded-Decky, and standalone installations must be separate distribution artifacts, not one artifact controlled by a writable update-channel marker. The Store artifact must exclude Homer-managed checking, downloading, installation, release-source configuration, automatic-update scheduling, and phone/CLI update routes; it shows the installed version only. Store release evidence must demonstrate that those capabilities are absent. A machine owner may deliberately replace installed software, but no paired phone, network request, or editable configuration may turn a Store artifact into a self-updating artifact.

Homer must not update Decky Loader or its plugin files through its non-Store path; if an agent update requires a newer Decky-facing component, it must fail safely and direct the user to Decky’s appropriate update mechanism.

## Threat model

Within a trusted LAN, Homer protects against accidental or unauthorised use by nearby devices that do not possess a valid paired-phone secret. Local approval, short-lived pairing, and the authenticated encrypted-message channel materially reduce unnoticed pairing, passive disclosure, replay, and unauthenticated message-injection risks.

Homer does not claim to protect against a hostile LAN participant, malicious access point/router, ARP/mDNS spoofing, or active traffic modification. HTTP makes browser-code replacement possible, and an attacker who changes the delivered interface can defeat its browser-side protections. Application-layer protection does not make public/shared Wi-Fi or internet exposure safe.

The project must continue to use least privilege, narrow fixed action allowlists, and deliberate confirmation for sensitive actions. Those measures reduce the impact of a compromised client but do not convert HTTP into secure transport.

## Privileged power boundary

The network-facing agent remains the normal graphical-session user. It uses stock logind interfaces when the operating system authorizes that context. Homer must not install a blanket polkit rule or treat phone confirmation as a substitute for required OS authorization.

The tested Bazzite background user-service context requires D-051/D-052’s optional, removable, fixed-purpose privileged power helper. Its installation, replacement, and removal require deliberate local elevation and cannot be requested by a phone or performed by Homer’s automatic updater. Store inclusion requires explicit Decky-maintainer acceptance; an artifact without the helper disables the unavailable power controls.

The helper is an on-demand local Python-standard-library service with no network listener. It accepts exactly one immediate `sleep`, `restart`, or `shutdown` operation through a small strictly validated, versioned JSON request on a filesystem-backed Unix stream socket from the configured Homer user, authenticated with socket permissions and kernel peer credentials. It has no generic verb framework, shell, caller-supplied command arguments, file-writing interface, sudoers fallback, polkit rule, or privileged cancellation method. Confirmation, countdown, and cancellation finish in the unprivileged agent before dispatch. The helper fails closed when absent, unhealthy, or incompatible. It maps the three operations to fixed non-interactive system-bus calls to the documented logind manager methods without caller-supplied arguments, a fallback dispatcher, or inhibitor-skipping flags.

The helper is a persistent root-owned component maintained only through a separate, locally initiated elevated operation. It cannot install, replace, roll back, or remove itself, and neither a phone, network request, automatic update, nor the rootless agent can initiate that maintenance. Complete versions, atomic selection, signed-manifest and exact-hash validation, harmless compatibility checks, rollback, and idempotent removal must remain independently recoverable without relying on code that runs after a power action. Bazzite uses the validated `/var/opt/homer/power-helper` persistent layout with normal SELinux policy and systemd-managed ephemeral coordination state under `/run`; Homer does not weaken the host security policy or modify the OS image to make the helper executable. SteamOS path, persistence, mount, and host-security behavior remain direct qualification requirements rather than inherited Bazzite assumptions.

This boundary does not distinguish Homer from every other process already running as the configured local user; such a process could request one of the three power actions. That local-account risk is explicit. The helper’s production package, installer and remover, service lifecycle and rate limits, hardening, IPC parser, peer check, compatibility behavior, audit trail, and SteamOS path remain release-review dependencies.

## Service and pairing availability

The independent agent may remain available to already paired phones after Decky fails. That does not mean pairing is always enabled.

- Only a local user action in Decky, initiated through the machine/controller UI, may enable pairing mode.
- No LAN request may enable pairing mode.
- Pairing mode lasts five minutes and then closes automatically.
- While pairing mode is disabled, the agent must reject pairing attempts and must not issue credentials.
- Pairing mode permits one pending request. A new local action is required to regenerate an expired, consumed, rejected, or failed bootstrap.
- A QR carries a 256-bit, single-use bootstrap secret in its URL fragment. The browser removes that fragment from visible URL/history state immediately after reading it. The bootstrap is used as a pre-shared handshake secret and is never sent as plaintext or reused as a control credential.
- A successful authenticated handshake binds the pending request and makes the bootstrap unavailable to further attempts.
- The phone and Decky display the same six-digit comparison code bound to that handshake. The target must approve or reject it locally within two minutes.
- Approval issues a unique 256-bit per-phone secret only inside the encrypted pairing channel.

This keeps the user experience analogous to deliberate Bluetooth pairing: the user asks the machine to become pairable, scans, approves locally, and then returns to ordinary use.

## Message protection and session authentication

Homer must use an established, independently reviewed pre-shared-key protocol family such as Noise to authenticate the phone and derive fresh authenticated-encryption keys for each session. Homer must not invent its own cryptographic handshake. The exact protocol pattern and pinned implementation depend on the approved agent/browser technology choices and are release dependencies, not latitude to omit protection.

The implementation must use cryptographically secure randomness, bundle all browser cryptography locally, load no executable code or assets from third parties, and enforce a restrictive Content Security Policy. It must pass the protocol’s official vectors, cross-implementation interoperability tests, malformed-message and replay tests, dependency/license review, and direct validation in supported mobile browsers. Protocol negotiation and version data must be bound into the authenticated handshake so they cannot be silently downgraded.

HTTP may expose only immutable browser assets and a content-free reachability response. Pairing is available only during the locally opened window. Every keyboard, mouse, clipboard, status, power, update, and remote-management message must travel through the authenticated encrypted channel; Homer must not provide a plaintext sensitive REST fallback.

Each WebSocket begins unauthenticated and must complete its handshake within ten seconds. Before completion it receives no sensitive state and may perform no action. Failed and malformed handshakes are closed and rate-limited without revealing whether a device identifier exists.

An authenticated phone explicitly acquires a thirty-second renewable control lease for input, clipboard, and power actions. The same phone may replace its stale prior connection immediately, which supports ordinary mobile reconnection. A different phone is clearly denied control until the holder releases it or the lease expires. v0.1 does not allow a remote kick-over or cross-phone handoff. Authenticated non-control capabilities must remain narrowly scoped to accepted actions such as status and the bounded update path.

## Credential lifecycle and browser storage

Each phone receives its own revocable 256-bit secret. The browser stores it in IndexedDB under Homer’s canonical origin; the agent keeps the credential-equivalent secret material in owner-only state. It is never placed in ordinary HTTP requests, URLs, logs, diagnostics, analytics, or telemetry. A browser/PWA must not claim that it can store the secret directly in native iOS Keychain or Android Keystore.

Credentials do not rotate or expire on a timer in v0.1. A local user may revoke any phone through Decky, and a connected phone may revoke only itself through its authenticated channel. Revocation immediately closes that phone’s connections and removes its agent-side credential. Intentional Homer uninstall removes all credentials and state.

Ordinary browser asset-cache clearing must not unpair a phone because the credential is held in IndexedDB, not the asset cache. Clearing or losing origin site data removes the browser’s only credential copy and requires the locally approved pairing flow again. The old agent-side record remains valid but inaccessible to that browser until the owner revokes it; the device-management UI must make stale records understandable. Homer must not substitute device fingerprinting, a URL-carried recovery credential, or another hidden identifier for local re-approval.

## Discovery, reconnection, and network binding

The pairing QR initially uses the agent’s current eligible RFC1918 IPv4 address, so first contact does not depend on mDNS. The agent also owns a stable, randomly generated, non-human-derived `.local` hostname. Canonical migration is eligible only on a supported configuration without a known relevant cross-mode failure and after the phone validates that hostname in its operating environment. A single success in one mode does not qualify a configuration known to fail in another supported mode. When qualified, a separate short-lived, single-use pre-shared migration claim moves the durable credential to the `.local` browser origin without putting the phone secret in a URL or plaintext request; successful migration removes the credential from the temporary IP origin.

When mDNS is unavailable or unqualified, Homer remains on the current IP origin in a visibly degraded discovery mode. It must explain that an address change can require a new locally approved connection. Homer does not retain the durable phone credential at both the IP and `.local` origins. A normal qualified canonical-origin reconnection uses the stored credential and stable hostname without a repeated QR scan or static-IP entry. The agent answers direct queries for its high-entropy hostname but does not advertise an enumerable Homer DNS-SD service; the hostname is a discovery identifier, not an authentication control. Silent recovery or automatic requalification after a later mode transition is not a v0.1 requirement.

The network service binds only to explicitly selected active RFC1918 LAN addresses, never wildcard, public, global-IPv6, VPN/tunnel, container, or other virtual-interface addresses. Loopback or local IPC may be used for machine-side management. The agent creates no UPnP, NAT-PMP, or other automatic router mapping. If no eligible address exists, Homer stays unavailable and reports why locally. IPv6-only, client-isolated, multicast-blocked without degraded direct-IP reachability, and segmented networks without deliberate local routing are outside the v0.1 compatibility claim.

Binding cannot defeat deliberate router port forwarding because forwarded traffic can arrive at the agent from an apparently local address. The user warning and trusted-LAN requirement therefore remain security controls, not mere documentation.

## First-run disclosure and defense in depth

Decky first run and each new-phone pairing show this meaning before the user continues:

> Homer is for a trusted home network. Control and clipboard messages are encrypted after pairing, but the web interface is loaded over HTTP. A hostile network can tamper with that interface and defeat this protection. Never port-forward Homer or use it on public or shared Wi-Fi.

The user explicitly continues once; normal phone UI retains a compact trusted-LAN status link without repeated interruptions.

Homer must minimise exposed surface area, reject invalid requests, rate-limit failed authentication, avoid permissive cross-origin access, log no sensitive payloads, and end inactive sessions. Static browser assets and cryptographic dependencies must be pinned and reviewable. Application-layer encryption must never be marketed as TLS, HTTPS, protection against an active hostile LAN, or permission to widen Homer’s network boundary.

## Release gate

Before claiming v0.1 readiness, test the five-minute pairing window, two-minute approval expiry, single-use bootstrap and migration claims, comparison-code binding, credential issuance/revocation, ten-second handshake deadline, encrypted-message/replay behavior, thirty-second control lease, same-phone reconnection, competing-phone rejection, cache-versus-site-data recovery, qualified mDNS canonical migration across claimed modes, degraded direct-IP behavior, eligible-interface binding, and no-internet-exposure behavior on supported configurations and mobile browsers. Validate stock graphical-user power authorization and, where required, the optional helper’s fixed allowlist, local-only IPC, package/install/uninstall behavior, and absence of a network listener or arbitrary-command path. Capture no real credentials, clipboard content, device names, or LAN addresses in public test artifacts.

The selected cryptographic implementation must pass official protocol vectors, independent interoperability tests, malformed-input tests, dependency review, and manual inspection before release. For a non-Store update path, test fixed-source enforcement, invalid signature/hash rejection, interrupted download handling, atomic activation, health-check rollback, active-session deferral, and the first-use disclosure. For a Store artifact, verify that the Homer update capability, release source, update scheduler, and phone/CLI update routes are absent. The first-run warning must be understandable without security expertise.
