# Release Guide

This standalone fork is being deprecated; see [DEPRECATION.md](DEPRECATION.md).
Deprecation does not require a new release. Preserve all existing tags, module
paths, and checksums so pinned consumers remain reproducible. Do not publish a
renamed module or retract otherwise valid releases as part of retirement.

The workflow remains available for maintenance releases while consumers are
reviewed. Use the process below only for maintenance work agreed in
[the retirement tracker](https://github.com/threshold-network/keep-common/issues/33).
Do not archive the repository until that review and the release-candidate rollout
are complete.

## Versioning
1) Use SemVer tags on `main`: `vX.Y.Z` when matching upstream versions; append `-tlabs.N` for fork-only releases (increment `N` for subsequent fork tags at the same base version).
2) Latest known upstream tag is `v1.7.0` from https://github.com/keep-network/keep-common (tracked via git tags). Upstream is unmaintained, so future releases proceed independently on this fork.

## Pre-release Checklist
1) Sync with upstream: pull the latest upstream tag/commit, resolve conflicts, and ensure CI is green.
2) Generators: `go generate ./.../gen`; verify the worktree is clean afterward.
3) Module sanity: `go mod tidy` (expect no diff) and `go list ./...` to confirm dependencies and packages resolve.
4) Quality gates: `go vet ./...`, `go test ./...`, and `govulncheck ./...` using the Go version in `go.mod`. Install the pinned scanner with `go install golang.org/x/vuln/cmd/govulncheck@v1.8.0`; add `go test -race ./...` for concurrency-heavy changes.
5) Changelog: update `CHANGELOG.md` with Added/Changed/Fixed/Breaking notes and mention the upstream commit/tag you synced.

## Tagging & Publishing
1) Tag: `git tag -a vX.Y.Z -m "Release vX.Y.Z"` (or `vX.Y.Z-tlabs.N` for fork-specific releases).
2) Push tag: `git push origin vX.Y.Z[-tlabs.N]`.
3) CI: pushing a `v*` tag triggers the release workflow to regenerate code, run vet/tests and blocking vulnerability checks, and publish a GitHub release with a placeholder body referencing `CHANGELOG.md`. Edit the GitHub release afterward to paste the changelog excerpt and upstream baseline notes.
