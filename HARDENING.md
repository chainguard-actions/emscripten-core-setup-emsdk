<!-- markdownlint-disable -->

# Hardening Report: emscripten-core--setup-emsdk/v12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **emscripten-core--setup-emsdk/v12** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow uses action references pinned to mutable tags or branch names instead of immutable full-length commit SHAs. This exposes the workflow to supply-chain attacks where a tag or branch is silently updated to malicious code. Failing references: `actions/checkout@v1` (line 8) and `mymindstorm/setup-emsdk@master` (lines 11, 18, 28, 36). All should be replaced with full 40-character commit SHAs.

Locations:

- `.github/workflows/checkin.yml:8`
- `.github/workflows/run.yml:11`
- `.github/workflows/run.yml:18`
- `.github/workflows/run.yml:28`
- `.github/workflows/run.yml:36`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within them declares job-level `permissions:` either. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions, which may be overly broad (e.g., write access to contents). Each workflow should declare minimal required permissions at the top level or per job.

Locations:

- `.github/workflows/checkin.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. checkin.yml: Pinned `actions/checkout@v1` → `actions/checkout@50fbc622fc4ef5163becd7fab6573eac35f8462e # v1`. Added top-level `permissions: contents: read`.

2. run.yml: Pinned all 4 occurrences of `mymindstorm/setup-emsdk@master` → `mymindstorm/setup-emsdk@0822153d7a5488b70a269cfa0a631b2a86ab4da2 # master` (note: the `master` branch does not exist on that repo; the SHA resolves to the HEAD of `main`). Added top-level `permissions: contents: read`.

