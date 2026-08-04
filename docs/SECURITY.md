# Security and connection model

## Status

This document preserves Homer’s accepted v0.1 security and connection intent. It is a product-security model, not an implementation protocol or a claim of formal security review. Implementation choices must conform to the accepted decisions in `docs/DECISIONS.md` and be validated before release.

## User promise and boundary

Homer is a local-network companion for a trusted home LAN. It has no cloud account, relay, or telemetry service, and it does not provide remote internet access. Its sole intended outbound-internet exception is checking for or fetching a verified Homer update for a non-Store installation, as described below. An official Decky Plugin Store installation has no Homer-managed update traffic; Decky owns that update path.

Homer v0.1 uses HTTP and WebSockets rather than HTTPS/WSS to avoid certificate warnings and certificate-management setup in mobile browsers. Consequently, it does **not** provide transport confidentiality or integrity against someone able to observe or alter the local network. Users must never port-forward Homer, expose it through a reverse proxy, enable automatic router exposure for it, or use it on an untrusted/shared network.

Homer must state this boundary plainly during first-run setup and in user-facing documentation. It must not intentionally create a public-internet listener or automatic router mapping.

## Non-Store update boundary

GitHub-sideloaded Decky and standalone Homer installations may check Homer’s fixed official release source for an update. A manual check is always available to an authorized paired phone or the local machine. On its first use, Homer may invite the user to enable automatic updates, but the prompt must not block the requested manual check.

Automatic behavior is opt-in and user-selectable: automatic checks, or automatic checks and installation, on a daily or weekly schedule. The default is manual-only. Automatic installation must wait until no remote control session is active. An authorized paired phone or the local machine may also explicitly request installation.

The updater is not a general downloader or remote-command feature. It must use only the fixed official source and release formats defined by Homer, authenticate release metadata and artifacts with a release-signing public key shipped with Homer, verify the declared artifact hash before activation, stage and validate a new version before atomically activating it, and automatically roll back to the last known-good version if the post-update health check fails. It must not accept a phone-supplied URL, executable, command, or arbitrary version path.

The update UI must disclose that the release source receives ordinary request metadata under that source’s privacy practices. Homer must not add its own device identifier, usage payload, installation-success ping, or heartbeat. Platform-provided aggregate release-download statistics remain downloads, not confirmed installations or successful updates.

Store, sideloaded-Decky, and standalone installations must be separate distribution artifacts, not one artifact controlled by a writable update-channel marker. The Store artifact must exclude Homer-managed checking, downloading, installation, release-source configuration, automatic-update scheduling, and phone/CLI update routes; it shows the installed version only. Store release evidence must demonstrate that those capabilities are absent. A machine owner may deliberately replace installed software, but no paired phone, network request, or editable configuration may turn a Store artifact into a self-updating artifact.

Homer must not update Decky Loader or its plugin files through its non-Store path; if an agent update requires a newer Decky-facing component, it must fail safely and direct the user to Decky’s appropriate update mechanism.

## Threat model

Within a trusted LAN, Homer protects against accidental or unauthorised use by nearby devices that do not possess a valid paired-phone credential. Local approval and short-lived pairing materially reduce the risk of an unnoticed nearby scan becoming control access.

Homer does not claim to protect against a hostile LAN participant, malicious access point/router, ARP/DNS spoofing, traffic capture, or traffic modification. HTTP makes these attacks possible. A bearer credential captured from HTTP traffic can be replayed; an attacker who can modify the delivered browser interface can also defeat browser-side protections.

The project must continue to use least privilege, narrow fixed action allowlists, and deliberate confirmation for sensitive actions. Those measures reduce the impact of a compromised client but do not convert HTTP into secure transport.

## Service and pairing availability

The independent agent may remain available to already paired phones after Decky fails. That does not mean pairing is always enabled.

- Only a local user action in Decky, initiated through the machine/controller UI, may enable pairing mode.
- No LAN request may enable pairing mode.
- Pairing mode is short-lived and automatically expires; its exact duration is an implementation parameter to validate.
- While pairing mode is disabled, the agent must reject pairing attempts and must not issue credentials.
- A short-lived, single-use QR bootstrap begins a pairing attempt. It is not a reusable control credential.
- The target must receive local Decky/controller approval before issuing a unique, revocable credential to the new phone.

This keeps the user experience analogous to deliberate Bluetooth pairing: the user asks the machine to become pairable, scans, approves locally, and then returns to ordinary use.

## Connected-session authorization

Each paired phone receives its own revocable bearer credential. Every control, clipboard, status, or power-action API requires authorization; only a minimal reachability check may be unauthenticated. A WebSocket must not accept or act on control data until the client authenticates at the application level.

Long-lived credentials and clipboard data must not be placed in URLs, browser history, logs, diagnostics, or telemetry. The exact handling of the short-lived QR bootstrap is still to be specified. Machine-side credential files must use restrictive owner-only permissions. Phone-side persistence remains an implementation question; a browser/PWA cannot directly claim to use native Keychain or Keystore APIs.

The exact credential format, rotation, expiration, WebSocket handshake mechanics, browser persistence, and revocation UX remain open design work. They must be specified before implementation.

## Defense in depth

Homer should minimise exposed surface area, reject invalid requests, rate-limit failed authorization, avoid permissive cross-origin access, avoid logging sensitive payloads, and end inactive sessions. Network binding and IPv4/IPv6 exposure must be validated on Bazzite and SteamOS.

Application-layer encrypted payloads may be investigated as a later, independently reviewed defense against passive observation after pairing. They must not be described as HTTPS-equivalent protection because the HTTP-delivered browser interface remains vulnerable to active network modification. Do not create a bespoke cryptographic protocol to solve this.

## Release gate

Before claiming v0.1 readiness, manually test the pairing-expiry, local-approval, credential revocation, unauthorized-request, and no-internet-exposure behaviors on the supported Bazzite and SteamOS configurations. For a non-Store update path, test fixed-source enforcement, invalid signature/hash rejection, interrupted download handling, atomic activation, health-check rollback, active-session deferral, and the first-use disclosure. For a Store artifact, verify that the Homer update capability, release source, update scheduler, and phone/CLI update routes are absent. The first-run warning must be understandable without security expertise.
