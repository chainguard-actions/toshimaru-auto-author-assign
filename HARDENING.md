<!-- markdownlint-disable -->

# Hardening Report: toshimaru--auto-author-assign/v3.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **toshimaru--auto-author-assign/v3.1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed CLAUDE.md without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/CLAUDE.md b/CLAUDE.md
deleted file mode 120000
index 47dc3e3..0000000
--- a/CLAUDE.md
+++ /dev/null
@@ -1 +0,0 @@
-AGENTS.md
\ No newline at end of file
diff --git a/CLAUDE.md b/CLAUDE.md
new file mode 100644
index 0000000..50ffd77
--- /dev/null
+++ b/CLAUDE.md
@@ -0,0 +1,59 @@
+# AI Agent Guide
+
+Guidance for any AI assistant working with this repository.
+
+## Project Overview
+
+**auto-author-assign** is a GitHub Action that automatically assigns pull request (and optionally issue) authors as assignees. When a PR or issue is opened/reopened, this action assigns the author to it.
+
+## Tech Stack
+
+- **Runtime**: Node.js 20+
+- **Build Tool**: Rollup (bundles source into single dist/index.js)
+- **Dependencies**: @actions/core, @actions/github
+- **Release**: release-please (automated via GitHub Actions)
+
+## Common Commands
+
+```bash
+# Install dependencies
+npm ci
+
+# Build the action (bundles src/index.js → dist/index.js)
+npm run package
+```
+
+**Releases**: Handled automatically by release-please. When PRs with conventional commits are merged to `main`, release-please creates/updates a release PR. Merging that PR triggers the release.
+
+## Project Structure
+
+```
+├── src/index.js       # Main action source code
+├── dist/index.js      # Bundled output (committed, auto-generated)
+├── action.yml         # GitHub Action metadata and inputs
+├── rollup.config.js   # Rollup bundler configuration
+└── .github/
+    └── workflows/
+        ├── build.yml         # CI: builds and checks dist is up-to-date
+        ├── ci.yml            # Tests the action on PRs
+        └── release-please.yml # Automated releases
+```
+
+## Key Development Notes
+
+- **Always rebuild before committing**: After modifying `src/index.js`, run `npm run package` and commit the updated `dist/index.js`. The CI will fail if dist is out of sync.
+- **Single source file**: All action logic is in `src/index.js` (~40 lines)
+- **No tests**: This project currently has no automated tests (`npm test` exits with error)
+- **GitHub access**: Use the `gh` CLI for any required GitHub interactions.
+
+## Action Behavior
+
+The action skips assignment when:
+1. Someone is already assigned as an assignee
+2. The author is a bot (type === "Bot")
+
+## Input Configuration
+
+| Input | Description | Default |
+|-------|-------------|---------|
+| `repo-token` | GitHub token for API calls | `${{ github.token }}` |

```

Locations:

- `CLAUDE.md`

## Iteration Notes

### Iteration 1

**Fixes applied:** prior-fix

**Notes:**

Re-applied the previous harden: wrote CLAUDE.md as a regular file (mode 0644) with the same content as AGENTS.md, replacing the symlink. This needs checking: after the write, list_directory still showed CLAUDE.md as a symlink to AGENTS.md, so write_file may have written through the link instead of replacing it. The content is the same either way, so AGENTS.md is unchanged. If the symlink is still there, the publish step has to delete it and add the regular file, because none of my tools can delete a file. No other files were changed.

