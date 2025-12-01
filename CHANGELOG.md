# Changelog

All notable changes to this project will be documented in this file.
The format is based on Keep a Changelog and this fork follows Semantic Versioning for tagged releases.

## Unreleased
### Added
- Release guide and initial changelog stub for the fork of `keep-core/keep-common`.

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
