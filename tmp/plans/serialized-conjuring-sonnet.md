# Plan: Add npm CLI 11.x Security Features

## Context

npm CLI 11.x (specifically v11.10.0) introduced three significant security features that are not yet fully reflected in this repository:

1. **`minimumReleaseAge`** — npm now officially supports this (no longer just a proposal)
2. **`--allow-git` flag** — controls git dependency execution during install
3. **`npm trust` bulk OIDC** — bulk configuration for trusted publishing

The article source: https://socket.dev/blog/npm-introduces-minimumreleaseage-and-bulk-oidc-configuration

PR #9 (bodadotsh/npm-security-best-practices#9) was about Bun's `minimumReleaseAge` and is already merged.

---

## Changes

### 1. Update npm `minimumReleaseAge` from "proposal" to shipped feature

**File:** `README.md` (line ~308)

Current text:
```
For `npm`, there is [a proposal](https://github.com/npm/cli/issues/8570) to add `minimumReleaseAge` option and `minimumReleaseAgeExclude` option.
```

Replace with text noting that npm CLI 11.x now supports `minimumReleaseAge`, with a link to the npm CLI 11.10.0 release. Also mention that exclusion support is not yet available (open issue: https://github.com/npm/cli/issues/8994).

Also add `minimumReleaseAge` usage in the `npm` command examples block around line ~296 (alongside the existing `--before` flag examples).

### 2. Add `--allow-git` flag information

**File:** `README.md` — in section "3. Disable Lifecycle Scripts" (around line ~221), after the existing content about `ignore-scripts`

Add a new subsection or note about git dependency execution risks:
- Git dependencies can include `.npmrc` that overrides the `git` executable path
- This can execute code even with `--ignore-scripts`
- `npm install --allow-git=none` prevents this
- Currently defaults to `all` for backward compatibility; expected to default to `none` in npm v12

### 3. Add `npm trust` to Trusted Publishing section

**File:** `README.md` — in section "Trusted Publishing" (around line ~487-498)

Add information about the `npm trust` command for bulk OIDC configuration:
- Allows adding/updating trusted publishing configs across multiple packages at once
- Useful for maintainers with large portfolios of packages
- Link to npm CLI 11.10.0 release and relevant docs

### 4. Update `.npmrc` config file

**File:** `.npmrc`

Add commented-out options in the "stricter control" section:
- `; minimum-release-age=<seconds>`
- `; allow-git=none`

---

## Files to modify

1. `README.md` — 3 edits (minimumReleaseAge update, --allow-git addition, npm trust addition)
2. `.npmrc` — add commented config options

## Verification

- Review the README rendering on GitHub to ensure links and formatting are correct
- Verify that the npm CLI release notes confirm the features: https://github.com/npm/cli/releases/tag/v11.10.0
