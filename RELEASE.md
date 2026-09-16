# Release Guide

Publish tagged releases of the `github.com/threshold-network/threshold-common`
Go module. This repository continues the history of `keep-common`; historical
tags keep their original module declarations and checksums.

## Versioning

Use immutable SemVer tags on `main`. The first release under the new module path
is planned as `v1.8.0`, after the repository rename and the checks in
[MIGRATION.md](MIGRATION.md). Confirm that the tag is unused before publishing.
Do not move or reuse the inherited `v1.7.0` or `v1.7.1-tlabs.*` tags. The old
`-tlabs.N` suffix records the fork's earlier releases; it is not required for
new Threshold releases.

## Pre-release checklist

1. Complete the coordinated repository/module cutover in [MIGRATION.md](MIGRATION.md).
2. Run `go generate ./...` and verify `git diff --exit-code`. This includes the
   generator templates themselves, as well as the promise fixtures.
3. Run `go mod tidy -diff` and `go list ./...`.
4. Run `go vet ./...`, `go build ./...`, `go test ./...`, and `govulncheck ./...`
   using the Go version in `go.mod`. Install the pinned scanner with
   `go install golang.org/x/vuln/cmd/govulncheck@v1.8.0`. Add `go test -race ./...`
   for concurrency changes.
5. Move the appropriate `CHANGELOG.md` entries into the release section,
   explicitly documenting the new module path and consumer migration.

## Tagging and publishing

1. From the reviewed commit on `main`, create an annotated `vX.Y.Z` tag and push
   that tag to the renamed repository.
2. The `v*` tag workflow regenerates code, runs vet/build/tests and vulnerability
   checks, and publishes a GitHub release. Replace its placeholder body with the
   changelog excerpt and migration link.
3. In a fresh module outside this checkout, run
   `go get github.com/threshold-network/threshold-common@vX.Y.Z` and verify
   `go list -m all`. Repeat with the default Go proxy/checksum database and with
   `GOPROXY=direct` to check both distribution paths.
4. Finalize downstream version pins and checksums only after the release is
   resolvable. Complete their builds and generator checks before merging.
