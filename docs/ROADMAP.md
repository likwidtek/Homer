# Roadmap

## Status

This is a collection of future and deferred ideas from product discussion. It is not a commitment to scope, timing, or technical approach.

## v0.1 focus

- Browser-based phone keyboard and trackpad/mouse for paired local machines.
- Clipboard transfer and convenient machine-to-phone clipboard retrieval within browser limitations.
- Restart, graceful shutdown, and sleep.
- Single actively paired and controlled machine, with architecture that does not preclude later multi-machine support.
- Decky-first installation through GitHub releases, bootstrapping an independent agent that continues to operate if Decky fails.
- Bazzite-first hands-on testing; SteamOS-aware design.

## Deferred: next product step

- Optional remote view / screen share of the machine, combining visual context with input and clipboard access.
- Assess a remote-view design without weakening the LAN-only, privacy-first boundary.

## v0.2 priority: Decky recovery

- Conservative Decky recovery, beginning with diagnosis, restart, and temporary disabling of third-party plugins to restore the Steam/Decky UI.
- Only after recovery is restored, offer confirmed, versioned, verified Decky repair/update actions.
- Validate every recovery action and its permissions on supported Bazzite and SteamOS configurations.

## Deferred: maintenance and recovery

- Force quit a frozen game or current game process.
- Useful maintenance actions for Steam, Flatpaks, the operating system, and Bazzite-specific update workflows.

## Deferred: game readiness

- Start an install or update a game so it is ready to play.

## Deferred: platform and distribution expansion

- Direct validation and supported release path for SteamOS.
- Broader support for Linux distributions, including immutable/atomic and conventional systems.
- Windows as a stretch/later target.
- Submit Homer to the official Decky Store only after the GitHub-delivered plugin is mature and Store eligibility has been confirmed.
- Additional distribution routes such as Bazzite integration, Flatpak, and other open-source stores or packaging methods.
- Confirm official Decky Store policy and eligibility, including its expectations for AI-assisted development and a plugin that bootstraps an independent agent.
- Easier installation without a physical keyboard/mouse, including controller-first paths where feasible.
- Independent installation for non-Decky users and recovery situations where Decky is unavailable.

## Deferred: multi-machine and Wake-on-LAN evolution

- Multiple fully supported paired machines.
- Research and select the minimum viable Wake-on-LAN path that is genuinely simple for phone-browser users.
- An awake paired-agent relay, optional dedicated local hub, or router integration for Wake-on-LAN only if it meets that ease-of-use bar.

## Guardrails for all future phases

- Preserve the browser-only phone experience unless the product direction is explicitly changed.
- Preserve the local-network-only, no-account, no-Homer-collected-telemetry, no-cloud-relay privacy model unless explicitly changed.
- Keep Decky optional so the companion remains useful when Decky is absent or broken.
- Keep every privileged action fixed, auditable, and confirmation-gated; do not add phone-accessible arbitrary shell execution.
- Aim for the normal network-facing agent to run rootlessly; validate its required permissions before claiming rootless support. If one-time setup or future recovery needs elevation, keep it narrowly scoped; isolate any privileged recovery helper so it does not listen on the network.
- Maintain maximum Decky Store safety compliance from the first GitHub release: concise/reviewable code, manual quality ownership, no self-updaters, no arbitrary code installation, no controller-input hijacking, and no broad destructive behavior.
- Before the first public release or accepting external pull requests, revisit a lightweight Homer trademark/brand policy, `CONTRIBUTING.md` with DCO sign-off, name clearance, and possible formal trademark registration.
