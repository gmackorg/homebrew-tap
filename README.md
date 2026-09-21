# gmackorg Homebrew tap

Command line tools published by [gmackorg](https://github.com/gmackorg).

```sh
brew install gmackorg/tap/<formula>
```

The `homebrew-` prefix is what makes `gmackorg/tap/...` resolve; you never type it.

## Formulae

| formula | what it is |
| --- | --- |
| `forgec` | the [ForgeGraph compiler](https://github.com/gmackie/forgec): one package, equivalent behaviour on Cloudflare, AWS and Node |

Formulae appear under `Formula/` as each project cuts its first release; the
table above lists what is on its way as well as what has landed.

## How these get here

This tap **pulls**; projects do not push into it. `bin/sync-formulae.mjs` runs
on a schedule here, reads `tap.json`, and regenerates each formula from the
latest GitHub release of the project that produces it.

That means no project needs a credential for this repository. A workflow here
already has write access here, and a public release is readable without auth,
so there is no personal access token or deploy key to mint, share, rotate, or
leak — and nothing to revoke when a project is retired.

Checksums are read from the `.sha256` files the release published, computed on
the machine that built the binary. They are never recomputed from a download,
which would only prove the download matched itself. A platform with no
published checksum is left out of the formula rather than guessed at.

A formula is never written by hand: edits here are overwritten by the next
sync. Fix it in the project that produces the release.

## Adding a CLI

Add an entry to [`tap.json`](tap.json) and the next sync picks it up:

```json
{ "name": "mytool", "repo": "owner/repo", "desc": "what it does", "license": "Apache-2.0" }
```

The project's release must attach `mytool-<version>-<target>.tar.gz` and a
sibling `.sha256` for each Rust target triple it supports, and tag as
`v<semver>`. Run the **Sync formulae** workflow manually to pick up a release
without waiting for the schedule.
