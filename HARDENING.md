<!-- markdownlint-disable -->

# Hardening Report: emscripten-core--setup-emsdk/v13

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **emscripten-core--setup-emsdk/v13** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved or compromised.

- `.github/workflows/checkin.yml`: `uses: actions/checkout@v1` (tag `v1`)
- `.github/workflows/run.yml`: `uses: mymindstorm/setup-emsdk@master` (branch `master`) — appears 4 times

All `uses:` references must be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v1`.

Locations:

- `.github/workflows/checkin.yml:8`
- `.github/workflows/run.yml:11`
- `.github/workflows/run.yml:18`
- `.github/workflows/run.yml:28`
- `.github/workflows/run.yml:35`

### missing-permissions (severity: medium)

Neither `.github/workflows/checkin.yml` nor `.github/workflows/run.yml` declares a top-level `permissions:` block, and no individual job within either file declares its own `permissions:` block. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often `write-all` for older repositories), granting jobs more access than necessary. Add a top-level `permissions: {}` (or minimal specific scopes) to each workflow file.

Locations:

- `.github/workflows/checkin.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v1 to SHA 50fbc622fc4ef5163becd7fab6573eac35f8462e in checkin.yml; (2) Pinned all 4 occurrences of mymindstorm/setup-emsdk@master to SHA 0822153d7a5488b70a269cfa0a631b2a86ab4da2 (the main branch HEAD, since the master branch no longer exists in that repo) in run.yml; (3) Added top-level `permissions: {}` to both checkin.yml and run.yml to enforce least-privilege token access.

