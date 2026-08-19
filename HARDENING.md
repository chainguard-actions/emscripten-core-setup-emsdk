<!-- markdownlint-disable -->

# Hardening Report: emscripten-core--setup-emsdk/v16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **emscripten-core--setup-emsdk/v16** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions by mutable tag or branch name instead of a pinned 40-character commit SHA. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to a malicious commit. Failing references: `actions/checkout@v6` (tag) and `emscripten-core/setup-emsdk@main` (branch, used 4 times).

Locations:

- `.github/workflows/checkin.yml:8`
- `.github/workflows/run.yml:14`
- `.github/workflows/run.yml:21`
- `.github/workflows/run.yml:33`
- `.github/workflows/run.yml:42`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no individual job within either file declares its own `permissions:` block. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (often `write-all` for older repositories), granting jobs more access than they need. Explicit minimal permissions should be declared at the top level or per job.

Locations:

- `.github/workflows/checkin.yml:1`
- `.github/workflows/run.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned actions/checkout@v6 to SHA d23441a48e516b6c34aea4fa41551a30e30af803 in checkin.yml; (2) Pinned all 4 occurrences of emscripten-core/setup-emsdk@main to SHA 0822153d7a5488b70a269cfa0a631b2a86ab4da2 in run.yml; (3) Added `permissions: {}` top-level block to both checkin.yml and run.yml to enforce least-privilege access.

