# gh-talooner

`gh` extension packaging for [`talooner`](https://github.com/opentalon/talooner),
the OpenTalon GitHub review bot's CLI. This repo carries no Go source of its
own — it repackages the binaries `talooner`'s own release workflow already
builds and attaches to its GitHub releases, under the asset naming `gh`
requires for precompiled extensions.

## Install

```bash
gh extension install opentalon/gh-talooner
```

Then either as a `gh` subcommand:

```bash
gh talooner cluster whoami
```

or, if the installed binary's directory is on `PATH` (see `gh extension list
--pinned` for its location), directly as `talooner` — same binary, same
subcommands. See `talooner`'s own [`auth.md`](https://github.com/opentalon/talooner/blob/master/auth.md)
for the full onboarding walkthrough.

## Version skew

The CLI dials the OpenTalon cluster's plugin over gRPC and is subject to the
same `protocol_version` floor as the Action (`talooner`'s `auth.md`,
`internal/cluster`'s `whoami` handshake) — a CLI built against a newer
protocol than the cluster's deployed plugin fails fast with one message
rather than misbehaving partway through a command. Landing order for any
protocol change is plugin first, tag, then bump callers — this repo included.

## Releasing

Releases here are **not** automatic. A maintainer runs the `Release` workflow
(`workflow_dispatch`) with the `talooner` git tag to package, e.g. `v1.2.0`.
The workflow downloads that tag's `talooner_<os>_<arch>` binaries from
[`talooner`'s release](https://github.com/opentalon/talooner/releases), renames
them to `gh-talooner-<os>-<arch>` per `gh`'s
[precompiled extension convention](https://docs.github.com/en/github-cli/github-cli/creating-github-cli-extensions),
and publishes a matching release here under the same tag.

Supported platforms: linux/amd64, linux/arm64, darwin/amd64, darwin/arm64.
No Windows build.
