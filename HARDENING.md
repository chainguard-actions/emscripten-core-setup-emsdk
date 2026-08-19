<!-- markdownlint-disable -->

# Hardening Report: emscripten-core--setup-emsdk/v14

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **emscripten-core--setup-emsdk/v14** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags or branch names instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced tag or branch is moved or compromised.

- `.github/workflows/checkin.yml` line 8: `uses: actions/checkout@v1` (tag `v1`)
- `.github/workflows/run.yml` line 14: `uses: mymindstorm/setup-emsdk@master` (branch `master`)
- `.github/workflows/run.yml` line 22: `uses: mymindstorm/setup-emsdk@master` (branch `master`)
- `.github/workflows/run.yml` line 32: `uses: mymindstorm/setup-emsdk@master` (branch `master`)
- `.github/workflows/run.yml` line 41: `uses: mymindstorm/setup-emsdk@master` (branch `master`)

All references should be pinned to a full 40-character hex commit SHA, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/checkin.yml:8`
- `.github/workflows/run.yml:14`
- `.github/workflows/run.yml:22`
- `.github/workflows/run.yml:32`
- `.github/workflows/run.yml:41`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often `write-all` for older repositories), granting jobs more access than necessary. A minimal `permissions:` block (e.g. `permissions: read-all` or specific scopes) should be added at the top level or per job.

Affected files:
- `.github/workflows/checkin.yml` — no `permissions:` key anywhere
- `.github/workflows/run.yml` — no `permissions:` key anywhere

Locations:

- `.github/workflows/checkin.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. checkin.yml: Pinned `actions/checkout@v1` to SHA `50fbc622fc4ef5163becd7fab6573eac35f8462e # v1`. Added top-level `permissions: contents: read`.

2. run.yml: Pinned all 4 occurrences of `mymindstorm/setup-emsdk@master` to SHA `0822153d7a5488b70a269cfa0a631b2a86ab4da2 # main` (the `master` branch does not exist in that repo; `main` is the current default branch). Added top-level `permissions: contents: read`.

