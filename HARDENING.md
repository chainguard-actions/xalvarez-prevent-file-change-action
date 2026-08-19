<!-- markdownlint-disable -->

# Hardening Report: xalvarez--prevent-file-change-action/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **xalvarez--prevent-file-change-action/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit.

automerge.yml:
  - uses: dependabot/fetch-metadata@v3.0.0 (line 16)

codeql-analysis.yml:
  - uses: actions/checkout@v6.0.2 (line 20)
  - uses: github/codeql-action/init@v4.35.1 (line 22)
  - uses: github/codeql-action/analyze@v4.35.1 (line 26)

test.yml:
  - uses: actions/checkout@v6.0.2 (line 12)
  - uses: actions/setup-node@v6.3.0 (line 16)
  - uses: actions/checkout@v6.0.2 (line 29)

All of these should be pinned to a full SHA, e.g. uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4

Locations:

- `.github/workflows/automerge.yml:16`
- `.github/workflows/codeql-analysis.yml:20`
- `.github/workflows/codeql-analysis.yml:22`
- `.github/workflows/codeql-analysis.yml:26`
- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:29`

### missing-permissions (severity: medium)

The workflow file test.yml has no top-level `permissions:` key, and the `build` job (line 8) has no job-level `permissions:` key. Without explicit permissions, the job inherits the default token permissions (which may be read/write depending on repository settings), violating the principle of least privilege. Only the `test` job has a job-level `permissions:` block. Add a top-level `permissions: {}` block (or explicit minimal scopes) and/or add `permissions:` to the `build` job.

Locations:

- `.github/workflows/test.yml:8`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Pinned all mutable action tags to full commit SHAs: dependabot/fetch-metadata@v3.0.0 → ffa630c65fa7e0ecfa0625b5ceda64399aea1b36, actions/checkout@v6.0.2 → de0fac2e4500dabe0009e67214ff5f5447ce83dd, github/codeql-action/init@v4.35.1 and github/codeql-action/analyze@v4.35.1 → c10b8064de6f491fea524254123dbe5e09572f13, actions/setup-node@v6.3.0 → 53b83947a5a98c8d113130e565377fae1a50d02f. Original tags preserved as inline comments. Added `permissions: {}` to the `build` job in test.yml to enforce least privilege (the job only runs npm ci and npm run all, requiring no GitHub token permissions).

