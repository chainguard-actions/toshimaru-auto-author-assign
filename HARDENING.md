<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag refs instead of full 40-character commit SHAs, making the workflows vulnerable to supply-chain attacks if the tag is moved.

- .github/workflows/build.yml: `uses: actions/checkout@v6` and `uses: actions/setup-node@v6`
- .github/workflows/ci.yml: `uses: actions/checkout@v6`
- .github/workflows/release-please.yml: `uses: googleapis/release-please-action@v4`

All of these should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v6`.

Locations:

- `.github/workflows/build.yml:11`
- `.github/workflows/build.yml:12`
- `.github/workflows/ci.yml:13`
- `.github/workflows/release-please.yml:12`

### missing-permissions (severity: medium)

The workflow file .github/workflows/build.yml has no top-level `permissions:` key and the single job `build` also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to all scopes). A minimal permissions block such as `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all unpinned action references by resolving them to full commit SHAs: actions/checkout@v6 → d23441a48e516b6c34aea4fa41551a30e30af803, actions/setup-node@v6 → 249970729cb0ef3589644e2896645e5dc5ba9c38, googleapis/release-please-action@v4 → 5c625bfb5d1ff62eadeeb3772007f7f66fdcf071. Original tags preserved as inline comments. Added top-level `permissions: contents: read` to build.yml to address the missing-permissions finding. ci.yml and release-please.yml already had explicit permissions blocks.

