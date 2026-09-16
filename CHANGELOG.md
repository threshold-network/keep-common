# Changelog

All notable changes to this project will be documented in this file.
The format is based on Keep a Changelog and this fork follows Semantic Versioning for tagged releases.

## Unreleased
### Deprecated
- Plan retirement of this standalone Threshold fork after keep-core's release candidate lands and remaining consumers are reviewed. Runtime code and generators already live in keep-core; preserve existing module/repository coordinates and immutable tags. See [DEPRECATION.md](DEPRECATION.md).

### Added
- Release guide and initial changelog stub for the fork of `keep-network/keep-common`.

### Changed
- Bump minimum Go toolchain to 1.26.8 to include standard library security fixes; CI and releases read this version from `go.mod`.
- Upgrade `github.com/ethereum/go-ethereum` to v1.17.3 and `github.com/gorilla/websocket` to v1.5.3 to address dependency advisories.
- Upgrade `govulncheck` to v1.8.0 and make vulnerability checks blocking in CI and before release publication (issue #3).

## Upstream Baseline - v1.7.0
### Notes
- Latest upstream tag from https://github.com/keep-network/keep-common (tracked via git tags); upstream is unmaintained, so this fork continues independently from that point.
### Added
- Inherited upstream history through v1.7.0 as the starting point for forked releases.
## v1.7.1-tlabs.0
### Changed
- Prefer block headers when retrieving block info to reduce RPC load and speed up block detection (PR #1).
- Regenerate promise async bindings to satisfy lint in CI.

### Added
- Tag-triggered release workflow to regenerate code, vet, test, and publish GitHub releases automatically.
