# Release Guide

Process for publishing tagged Go module releases for this fork of `keep-common`.

## Versioning
1) Use SemVer tags on `main`: `vX.Y.Z` when matching upstream versions; append `-tlabs.N` for fork-only releases (increment `N` for subsequent fork tags at the same base version).
2) Before tagging, check the latest upstream `keep-core/keep-common` tag to avoid collisions and decide whether you are mirroring or diverging.

## Pre-release Checklist
1) Sync with upstream: pull the latest upstream tag/commit, resolve conflicts, and ensure CI is green.
2) Generators: `go generate ./.../gen`; verify the worktree is clean afterward.
3) Module sanity: `go mod tidy` (expect no diff) and `go list ./...` to confirm dependencies and packages resolve.
4) Quality gates: `go vet ./...` and `go test ./...`; add `go test -race ./...` for concurrency-heavy changes.
5) Changelog: update `CHANGELOG.md` with Added/Changed/Fixed/Breaking notes and mention the upstream commit/tag you synced.

## Tagging & Publishing
1) Tag: `git tag -a vX.Y.Z -m "Release vX.Y.Z"` (or `vX.Y.Z-tlabs.N` for fork-specific releases).
2) Push tag: `git push origin vX.Y.Z[-tlabs.N]`.
3) Create a GitHub release from the tag with the changelog excerpt and a note about the upstream baseline.
