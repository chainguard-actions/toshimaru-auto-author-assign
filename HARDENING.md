<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v2.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v2.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable tags instead of immutable full-length SHA commit hashes. In build.yml: `actions/checkout@v6` and `actions/setup-node@v6` are tag-based refs. In ci.yml: `actions/checkout@v6` is a tag-based ref. These can be silently updated by the action author, enabling supply-chain attacks.

Locations:

- `.github/workflows/build.yml:12`
- `.github/workflows/build.yml:13`
- `.github/workflows/ci.yml:14`

### missing-permissions (severity: medium)

Workflow file build.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow runs with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/build.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings in build.yml and ci.yml:
1. unpinned-uses: Pinned `actions/checkout@v6` → `actions/checkout@d23441a48e516b6c34aea4fa41551a30e30af803 # v6` in both build.yml (line 12) and ci.yml (line 14). Pinned `actions/setup-node@v6` → `actions/setup-node@249970729cb0ef3589644e2896645e5dc5ba9c38 # v6` in build.yml (line 13).
2. missing-permissions: Added `permissions: {}` top-level block to build.yml. The ci.yml already had explicit permissions (issues: write, pull-requests: write) so no change was needed there.

