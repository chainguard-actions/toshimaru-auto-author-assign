<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag refs instead of full 40-character SHA commit digests, making them vulnerable to supply-chain attacks if the referenced tags are moved or hijacked.

- .github/workflows/build.yml: `uses: actions/checkout@v6` and `uses: actions/setup-node@v6`
- .github/workflows/ci.yml: `uses: actions/checkout@v6`
- .github/workflows/release-please.yml: `uses: googleapis/release-please-action@v5`

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v6`.

Locations:

- `.github/workflows/build.yml:12`
- `.github/workflows/build.yml:13`
- `.github/workflows/ci.yml:15`
- `.github/workflows/release-please.yml:13`

### missing-permissions (severity: medium)

The workflow file .github/workflows/build.yml has no top-level `permissions:` key and its only job (`build`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository token permissions, which may be overly broad (write access to contents by default on some repository configurations). A minimal permissions block such as `permissions: read-all` or specific scopes should be added.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all four mutable action tag references to full 40-character SHA digests (with original tags preserved in comments): actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → 249970729cb0ef3589644e2896645e5dc5ba9c38, googleapis/release-please-action@v5 → 45996ed1f6d02564a971a2fa1b5860e934307cf7. Added a top-level `permissions: contents: read` block to build.yml, which is the minimum required for the checkout and build steps.

