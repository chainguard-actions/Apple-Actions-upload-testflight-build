<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--upload-testflight-build/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--upload-testflight-build/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks where a tag can be silently moved to point to malicious code. Failing references: actions/checkout@v6.0.0, actions/setup-node@v5.0.0, actions/upload-artifact@v5.0.0. Each should be pinned to a full SHA, e.g. actions/checkout@<40-hex-sha> # v6.0.0.

Locations:

- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:29`
- `.github/workflows/check-dist.yml:50`
- `.github/workflows/eslint.yml:20`
- `.github/workflows/eslint.yml:26`
- `.github/workflows/knip.yml:19`
- `.github/workflows/knip.yml:25`
- `.github/workflows/prettier.yml:20`
- `.github/workflows/prettier.yml:26`
- `.github/workflows/test.yml:19`
- `.github/workflows/test.yml:25`

### missing-permissions (severity: medium)

None of the workflow files declare a top-level 'permissions:' block, and no job within any workflow has a job-level 'permissions:' block. Without explicit permissions, workflows run with the default token permissions (which may include write access to contents, packages, etc.), violating the principle of least privilege. Each workflow should declare minimal required permissions, e.g. 'permissions: read-all' at minimum, or specific scopes such as 'contents: read'.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/knip.yml:1`
- `.github/workflows/prettier.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 workflow files (check-dist.yml, eslint.yml, knip.yml, prettier.yml, test.yml):
1. unpinned-uses: Pinned all action references to full 40-char SHAs with tag comments preserved:
   - actions/checkout@v6.0.0 → @1af3b93b6815bc44a9784bd300feb67ff0d1eeb3 # v6.0.0
   - actions/setup-node@v5.0.0 → @a0853c24544627f65ddf259abe73b1d18a591444 # v5.0.0
   - actions/upload-artifact@v5.0.0 → @330a01c490aca151604b8cf639adc76d48f6c5d4 # v5.0.0
2. missing-permissions: Added top-level 'permissions: contents: read' block to all 5 workflows. The workflows only need to read repository contents (checkout), so contents: read is the minimal required permission.

