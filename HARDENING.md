<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--upload-testflight-build/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--upload-testflight-build/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All four workflow files reference GitHub Actions using mutable version tags instead of pinned full SHA commit hashes. This exposes the action to supply-chain attacks where a tag could be moved to point to malicious code. Failing references:
- actions/checkout@v4.2.2
- actions/setup-node@v4.4.0
- actions/upload-artifact@v4.6.2
Each should be replaced with the corresponding 40-character commit SHA (e.g., `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2`).

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/eslint.yml:18`
- `.github/workflows/eslint.yml:23`
- `.github/workflows/knip.yml:18`
- `.github/workflows/knip.yml:23`
- `.github/workflows/prettier.yml:18`
- `.github/workflows/prettier.yml:23`

### missing-permissions (severity: medium)

None of the four workflow files define a top-level `permissions:` block, and none of the individual jobs define their own `permissions:` block. Without explicit permissions, workflows run with the default repository permissions (which may include write access to contents, packages, etc.), violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/knip.yml:1`
- `.github/workflows/prettier.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Updated all four workflow files (.github/workflows/check-dist.yml, eslint.yml, knip.yml, prettier.yml):
1. unpinned-uses: Replaced mutable version tags with full 40-character commit SHAs — actions/checkout@v4.2.2 → @11bd71901bbe5b1630ceea73d27597364c9af683, actions/setup-node@v4.4.0 → @49933ea5288caeca8642d1e84afbd3f7d6820020, actions/upload-artifact@v4.6.2 → @ea165f8d65b6e75b540449e92b4886f43607fa02. Version tags preserved as inline comments.
2. missing-permissions: Added top-level `permissions: contents: read` block to all four workflow files, granting only the minimum access needed for checkout and build operations.

