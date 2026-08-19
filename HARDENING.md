<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags instead of pinned full 40-character SHA digests, making them vulnerable to supply-chain attacks if the tag is moved.

- `.github/workflows/build.yml`: `actions/checkout@v6` and `actions/setup-node@v6`
- `.github/workflows/ci.yml`: `actions/checkout@v6`
- `.github/workflows/release-please.yml`: `googleapis/release-please-action@v4`

All of these should be pinned to a full commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/build.yml:12`
- `.github/workflows/build.yml:13`
- `.github/workflows/ci.yml:14`
- `.github/workflows/release-please.yml:12`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/build.yml` has no top-level `permissions:` key and its only job (`build`) also has no job-level `permissions:` key. Without explicit permissions, the workflow inherits the default repository permissions (which may include broad write access). A minimal `permissions: read-all` or specific scopes (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four unpinned action references by resolving their full commit SHAs via lookup_action_sha and updating them in the format 'owner/repo@<sha> # tag'. Added a top-level 'permissions: contents: read' block to build.yml, which is the minimum permission needed for a workflow that checks out code and runs npm build commands. The ci.yml and release-please.yml already had explicit permissions blocks so no changes were needed there.

