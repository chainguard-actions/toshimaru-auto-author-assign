<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v3.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tag refs instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the tag is moved.

build.yml:
- `uses: actions/checkout@v7` (line 13)
- `uses: actions/setup-node@v7` (line 14)

ci.yml:
- `uses: actions/checkout@v7` (line 14)

release-please.yml:
- `uses: googleapis/release-please-action@v5` (line 13)

Locations:

- `.github/workflows/build.yml:13`
- `.github/workflows/build.yml:14`
- `.github/workflows/ci.yml:14`
- `.github/workflows/release-please.yml:13`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/build.yml` has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be broader than necessary (e.g., write access to contents). A minimal `permissions:` block such as `contents: read` should be added.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all four unpinned action references to full 40-character SHA hashes with tag comments: actions/checkout@v7 → SHA 3d3c42e (build.yml and ci.yml), actions/setup-node@v7 → SHA 8207627 (build.yml), googleapis/release-please-action@v5 → SHA 45996ed (release-please.yml). Added top-level `permissions: contents: read` block to build.yml to satisfy the missing-permissions finding. The ci.yml and release-please.yml already had appropriate permissions blocks.

