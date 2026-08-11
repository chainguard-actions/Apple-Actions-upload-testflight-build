<!-- markdownlint-disable -->

# Hardening Report: Apple-Actions--upload-testflight-build/v5.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Apple-Actions--upload-testflight-build/v5.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### missing-permissions (severity: medium)

The workflow file has no top-level 'permissions:' key and no job-level 'permissions:' key on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (often broad) permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/eslint.yml:1`
- `.github/workflows/knip.yml:1`
- `.github/workflows/prettier.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** missing-permissions

**Notes:**

Added `permissions: contents: read` top-level block to all five workflow files: check-dist.yml, eslint.yml, knip.yml, prettier.yml, and test.yml. All workflows only perform read operations (checkout, build, lint, test), so `contents: read` is the minimum necessary permission. This replaces the implicit broad default GITHUB_TOKEN permissions with explicit least-privilege permissions.

