<!-- markdownlint-disable -->

# Hardening Report: xalvarez--prevent-file-change-action/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xalvarez--prevent-file-change-action/v3.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three workflow files use mutable version tags instead of pinned full 40-character SHA commit hashes for their `uses:` references. This exposes the action to supply-chain attacks where a compromised or updated tag could execute malicious code.

- automerge.yml: `dependabot/fetch-metadata@v2.4.0`
- codeql-analysis.yml: `actions/checkout@v5.0.0`, `github/codeql-action/init@v3.30.5`, `github/codeql-action/analyze@v3.30.5`
- test.yml: `actions/checkout@v5.0.0`, `actions/setup-node@v5.0.0`

All should be pinned to their full 40-character SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v5.0.0`.

Locations:

- `.github/workflows/automerge.yml:14`
- `.github/workflows/codeql-analysis.yml:17`
- `.github/workflows/codeql-analysis.yml:19`
- `.github/workflows/codeql-analysis.yml:22`
- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:16`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all mutable version tag references to full 40-character SHA commit hashes across three workflow files:
- automerge.yml: dependabot/fetch-metadata@v2.4.0 → @08eff52bf64351f401fb50d4972fa95b9f2c2d1b
- codeql-analysis.yml: actions/checkout@v5.0.0 → @08c6903cd8c0fde910a37f88322edcfb5dd907a8; github/codeql-action/init and /analyze@v3.30.5 → @3599b3baa15b485a2e49ef411a7a4bb2452e7f93
- test.yml: actions/checkout@v5.0.0 (both occurrences) → @08c6903cd8c0fde910a37f88322edcfb5dd907a8; actions/setup-node@v5.0.0 → @a0853c24544627f65ddf259abe73b1d18a591444
Original version tags preserved as inline comments for readability.

