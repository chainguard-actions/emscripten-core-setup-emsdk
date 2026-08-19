<!-- markdownlint-disable -->

# Hardening Report: emscripten-core--setup-emsdk/v15

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **emscripten-core--setup-emsdk/v15** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions and reusable workflows by mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit.

.github/workflows/checkin.yml:
  - Line 8: `uses: actions/checkout@v1` (tag `v1` is mutable)

.github/workflows/run.yml:
  - Line 14: `uses: mymindstorm/setup-emsdk@master` (branch `master` is mutable)
  - Line 22: `uses: mymindstorm/setup-emsdk@master` (branch `master` is mutable)
  - Line 33: `uses: mymindstorm/setup-emsdk@master` (branch `master` is mutable)
  - Line 42: `uses: mymindstorm/setup-emsdk@master` (branch `master` is mutable)

All `uses:` references should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v1`.

Locations:

- `.github/workflows/checkin.yml:8`
- `.github/workflows/run.yml:14`
- `.github/workflows/run.yml:22`
- `.github/workflows/run.yml:33`
- `.github/workflows/run.yml:42`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and none of the individual jobs define job-level `permissions:` blocks. Without explicit permissions, GitHub Actions grants the default token permissions (which may include `write` access to repository contents and other scopes depending on the organization/repository settings). Explicit minimal permissions should be declared.

.github/workflows/checkin.yml: No top-level or job-level `permissions:` key. Job `check_pr` has no permissions block.

.github/workflows/run.yml: No top-level or job-level `permissions:` key. Jobs `defaults`, `actions-use-cache-test`, `no-install`, and `tot` have no permissions blocks.

Locations:

- `.github/workflows/checkin.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. checkin.yml: Pinned `actions/checkout@v1` to full SHA `50fbc622fc4ef5163becd7fab6573eac35f8462e`. Added top-level `permissions: {}` and job-level `permissions: contents: read` for the check_pr job.

2. run.yml: Pinned all 4 occurrences of `mymindstorm/setup-emsdk@master` to full SHA `0822153d7a5488b70a269cfa0a631b2a86ab4da2` (the HEAD of the `main` branch — note: `master` branch does not exist in that repo, `main` is the default). Added top-level `permissions: {}` and job-level `permissions: contents: read` for all 4 jobs (defaults, actions-use-cache-test, no-install, tot).

