# Deprecating the standalone Threshold fork

`threshold-network/keep-common` is being retired as a standalone library. Keep
its existing repository name and `github.com/keep-network/keep-common` module
identity. There will be no `threshold-common` rename or replacement module.

This decision applies only to the Threshold fork. The original
`keep-network/keep-common` repository and other organizations' forks have their
own owners and maintenance policies.

## Why keep-core no longer needs it

At the reviewed release-candidate commit
[`a90ce1bf0738a35e4e3881f634f07520c2b8529e`](https://github.com/threshold-network/keep-core/tree/a90ce1bf0738a35e4e3881f634f07520c2b8529e),
keep-core has incorporated both the runtime packages and code generators from
this repository through [PR #4327](https://github.com/threshold-network/keep-core/pull/4327).
Its `go.mod`, `go.sum`, imports, and generator commands no longer consume this
module. The sole remaining `keep-common` string is a historical link to the
original upstream issue #117, not a build dependency.

The final incorporated package layout is not `pkg/keepcommon`. Runtime packages
were flattened into keep-core (for example `pkg/persistence` and
`pkg/chain/ethereumutil`), and the generator is local at
`tools/generators/ethereum`. There is no supported one-line module replacement
for other applications: owners need to review their own API and build needs.

The migration baseline is **dev**, which is expected to land on main through
[PR #4256](https://github.com/threshold-network/keep-core/pull/4256) before this
retirement is completed. Older main still pins this fork; verify the actual
landed release rather than adding a temporary rename migration to old main.

## Remaining consumers

A review on 2026-09-16 found additional organization-private release/tooling
repositories with replacement directives pinning this fork's
`v1.7.1-tlabs.0` or `v1.7.1-tlabs.1`, plus operational repository inventories.
Their owners must classify each use as an active maintained build or a retained
historical snapshot. The detailed inventory is tracked privately.

For active keep-core forks, prefer carrying forward the reviewed release
candidate's in-tree packages and generators through their normal release merge.
For independent tools, review actual imports and tests before pruning unused
requirements or choosing maintained alternatives. Historical snapshots may keep
their exact pins and checksums; deprecation is not a reason to modify recorded
release inputs.

Default-branch code search is not an exhaustive dependency graph. Check
maintained branches, nested Go modules, build scripts, and automation before
closing the consumer review. A consumer of original Keep upstream or another
organization's fork is not automatically a consumer of this Threshold fork.

## Retirement sequence

The checklist and owner decisions are tracked in
[issue #33](https://github.com/threshold-network/keep-common/issues/33).

1. Confirm the keep-core release candidate has landed on main and that neither
   runtime code nor generators reacquire this external dependency.
2. Record retain/migrate decisions for every remaining maintained consumer and
   historical snapshot. Validate changed consumers with their normal release
   inputs and tests; record the support expectations for intentionally retained
   pins.
3. Keep this repository and all tags, commits, and releases available. Do not
   rename it, delete/reuse tags, hand-edit checksums, or retract valid versions
   solely to announce retirement.
4. Keep CI and dependency monitoring active while the review is open. Any
   maintenance release must be agreed with the affected consumer owners; new
   development of the absorbed code belongs in keep-core.
5. Once the release and consumer decisions are complete, resolve outstanding
   work and adjust external repository inventories/monitoring. An administrator
   can then archive the repository. Archiving is separate from merging this
   documentation and retains the Git contents for existing builds.

## Go tooling and publication

This repository contains one root Go module, including the generators; there is
no separate npm, Python, Rust, or container publication configured here. The
release workflow creates GitHub releases from `v*` tags.

A new release is not required for this documentation notice. Because this fork
still declares the original upstream module path and consumers commonly reach
it through `replace`, do not assume that a `Deprecated:` comment here would
advertise this fork's status to every consumer of the original module. If a
Go-tool-visible notice is wanted, verify its behavior for the actual replacement
and version-selection paths and agree any final maintenance tag separately.
Do not publish a new module identity merely to deprecate it.

References: [Go module deprecation and replacement rules](https://go.dev/ref/mod),
[GitHub archive behavior](https://docs.github.com/en/repositories/archiving-a-github-repository/archiving-repositories).
