# Migrating to threshold-common

The new repository and Go module are
`github.com/threshold-network/threshold-common`. Package names and exported APIs
are unchanged by the rebrand, but import paths are part of Go package identity.
Update an application's complete dependency graph consistently.

There is one root Go module in this repository, including the command-line code
generators. There is no npm, Python, Rust, or container publication configured
here. GitHub releases are published by `.github/workflows/release.yml`.

## Release order

1. Review and test the module/import/template changes while the repository is
   still named `threshold-network/keep-common`.
2. An administrator renames that repository to `threshold-common`. Coordinate
   this with merging the rebrand; the new module cannot be fetched from its new
   location before the repository exists there. Verify repository integrations,
   branch protections, scheduled jobs, and release permissions after the rename.
3. Update local remotes, badges, and external automation. Preserve GitHub's old
   repository redirect: do not create another `threshold-network/keep-common`
   repository. Keep all historical tags unchanged.
4. Publish the first new-path release (planned `v1.8.0`) using [RELEASE.md](RELEASE.md).
   Validate a clean external download through the Go proxy/checksum database and
   directly from GitHub before advancing consumers.
5. Finalize and merge downstream PRs, including `go.mod`, `go.sum`, generated
   bindings, and generator invocations. An unreleased version or a local testing
   replacement must not be merged into a consumer's release branch.

GitHub redirects repository URLs, but that does not change the `module`
declaration in a tagged commit. Old tags still declare
`github.com/keep-network/keep-common`; they are not releases of the new module.
The original upstream `keep-network/keep-common` is a separate repository, not
an alias for this Threshold fork. Do not redirect its historical issue links to
this repository.

## Consumer changes

- Replace `github.com/keep-network/keep-common` imports and generator commands
  with `github.com/threshold-network/threshold-common`.
- Remove any replacement such as
  `github.com/keep-network/keep-common => github.com/threshold-network/keep-common`.
- Require the published new-path release directly. Run `go mod tidy` to generate
  authentic checksums; do not rename existing `go.sum` entries by hand.
- Regenerate contract/command bindings from their ABI inputs with the renamed
  generator. Update `.go.tmpl` sources and their embedded Go templates together.
- Verify `go list -m all` and `go list -deps ./...` contain no unintended old
  common module. A mixed graph can contain distinct named Go types even when
  source declarations match. A `replace` in this library's `go.mod` would not
  propagate to consumers and is not a compatibility solution.
- Match the minimum Go version in this module's `go.mod` (currently 1.26.8) in
  consumer builds, CI, and Docker builders. Account for the existing dependency
  upgrade to go-ethereum 1.17.3 when moving from an older common release.

For pre-release testing, use a temporary `go.work` or an uncommitted local
replacement pointing to the reviewed common checkout. This validates source
compatibility only; it does not validate the future published version or its
checksums. Remove the testing override and repeat validation after publication.

## Downstream baseline

The migration baseline is `threshold-network/keep-core` **dev**, the release
candidate that is expected to merge to main before this rename is actioned.
As verified on 2026-09-16 at `a90ce1bf0738a35e4e3881f634f07520c2b8529e`:

- [PR #4327](https://github.com/threshold-network/keep-core/pull/4327) absorbed
  both runtime packages and generators into keep-core. `go.mod`, `go.sum`,
  imports, and generator invocations no longer depend on this repository.
- The sole remaining `keep-common` reference is a link to original upstream
  issue #117 in the tBTC generator Makefile. It describes a historical ABI
  workaround and should stay unchanged.
- [PR #4256](https://github.com/threshold-network/keep-core/pull/4256) tracks the
  release to main. Verify it has landed before the cutover. The older main
  branch still uses a replacement at `v1.7.1-tlabs.0`; do not reintroduce that
  external dependency into dev just to rename it.

Search maintained branches, private build tooling, and external consumers before
closing the cutover issue: GitHub code search primarily covers default branches
and is not proof that every consumer has been found. Consumers of the original
upstream or of another organization's fork are not automatically consumers of
this Threshold fork. Historical release snapshots should retain their pins
unless their owners explicitly choose to migrate them.

## References

- [Go module paths and replacement rules](https://go.dev/ref/mod)
- [GitHub repository rename behavior](https://docs.github.com/en/repositories/creating-and-managing-repositories/renaming-a-repository)
