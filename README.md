# Homer

Homer is an open-source, privacy-first companion for couch-gaming Linux machines. It lets a paired phone's web browser provide quick keyboard and trackpad/mouse input on the local network, avoiding the need to reach for physical peripherals.

## Project status

Homer is currently in product design. It is not yet installable and no application implementation has begun.

## Direction for v0.1

- Browser-only phone client; no required native iPhone or Android app.
- One paired local machine with keyboard and trackpad/mouse input.
- Explicit phone-to-machine text transfer and convenient machine-to-phone clipboard retrieval, within browser limitations.
- Restart, graceful shutdown, and sleep.
- QR pairing with short-lived bootstrap, local target approval, and revocable phone credentials.
- Decky-first installation through GitHub releases, bootstrapping an agent that remains functional if Decky later fails.
- Bazzite and SteamOS as required initial targets, with Bazzite-first hands-on testing and direct SteamOS validation before release claims.

## Safety and privacy

Homer is local-network-only. It has no accounts, cloud relays, remote internet access, or Homer-collected telemetry. The project may use aggregate repository traffic and release-download counts supplied by distribution platforms; those are not install or active-user metrics.

The phone client will never provide arbitrary shell or terminal access. Privileged behavior, if needed, is limited to fixed, audited, deliberately confirmed actions. The normal agent aims to run rootlessly; required permissions remain to be validated.

## Not in v0.1

- Screen sharing or remote desktop
- Multiple actively controlled machines
- Wake-on-LAN
- Decky recovery or repair actions
- System, application, or game updates
- Windows support
- An official Decky Plugin Store listing

See [the product direction](docs/PRODUCT.md), [recorded decisions](docs/DECISIONS.md), and [roadmap](docs/ROADMAP.md) for details.

## Publishing safety

The public repository has a required local pre-push gate for private-data and credential checks. See [publishing and public-repository safety](docs/PUBLISHING.md).

## Contributing

The project is not yet accepting external pull requests. Before that changes, Homer will publish contributor guidance with DCO sign-off requirements.

## License

Copyright © 2026 likwidtek.

Homer is licensed under the GNU General Public License, version 3 or any later version. See [LICENSE](LICENSE).
