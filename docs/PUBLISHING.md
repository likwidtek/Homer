# Publishing and public-repository safety

## Status

Homer is publicly hosted at `https://github.com/likwidtek/Homer`. The `main` branch is the public source of truth. `docs/SESSION.md` is intentionally local-only and must never be committed or pushed.

## Hard pre-push requirement

Every push must pass the versioned pre-push hook. Do not use `git push --no-verify`.

The hook fails closed when:

- The working tree or index has uncommitted changes.
- Git detects whitespace errors.
- A tracked path matches a forbidden private-data pattern, including `docs/SESSION.md`, `.env` files, keys, certificates, tokens, or `secrets/` and `credentials/` directories.
- Gitleaks is unavailable.
- Gitleaks finds a potential secret in Git history.

Install the required scanner and activate the repository-local hook once per clone:

```sh
brew install gitleaks
git config --local core.hooksPath .githooks
```

Run the same gate manually before a significant push:

```sh
scripts/preflight-push
```

The hook is a deliberate local guardrail, not an authorization system: a user can technically bypass Git hooks. Do not bypass it. GitHub secret scanning and push protection are required independent backstops; they detect supported credential patterns but cannot guarantee detection of every private datum or custom secret.

## Human review checklist

Before a push, confirm:

1. The change belongs in the public repository and contains no personal, network, device, clipboard, or customer data.
2. No credentials, pairing data, private keys, certificates, tokens, `.env` files, logs, screenshots, or captures are staged.
3. The change has been read as a reviewer would read it; generated code has not bypassed security review.
4. Privileged behavior remains fixed, allowlisted, locally auditable, and confirmation-gated.
5. Dependencies, downloads, and update behavior are explicit, versioned, and reviewable.
6. The pre-push gate passes without bypass.

## GitHub settings

Keep the repository public and enable GitHub Secret Protection, including secret scanning and push protection. Enable Dependabot alerts and code scanning once the project contains dependencies and application code. Do not rely on these services as the only defense.

Before accepting contributors, add a `SECURITY.md`, configure private vulnerability reporting, publish `CONTRIBUTING.md` with DCO sign-off, and protect `main` with an appropriate review and CI policy.
