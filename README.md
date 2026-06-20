# seed-sync-binaries

Public **release artifacts** for **S.E.E.D. (Seed Sync)** — the Linux tarball and
the Windows installer. This repo holds binaries only; the source lives in the
**private** [`seed-sync-gtk`](https://github.com/steeb-k/seed-sync-gtk) repo.

## How it works
- Each release is a **GitHub Release** tagged `vX.Y.Z`, carrying both platforms'
  assets:
  - `seed-sync-<ver>-linux-x86_64.tar.gz` — published automatically by the
    `seed-sync-gtk` **release workflow** on a `v*` tag.
  - the Windows installer (MSI/zip) — attached to the same release by the Windows side.
- It's **public on purpose**: the install/update tooling downloads assets with no
  authentication. Publishing *to* this repo requires a token (`SEED_BINARIES_TOKEN`,
  stored as a secret in `seed-sync-gtk`); downloading does not.

## Installing / updating
See the latest release's assets, or the install instructions in `seed-sync-gtk`
(`README.md` / `docs/linux-packaging.md`). On Linux the daily `seed-sync-update`
timer keeps installs current automatically.

## Maintainers
- **Do not commit source or large files here** — only release assets, attached via
  Releases (not committed to the tree).
- Releases are cut by tagging `vX.Y.Z` in `seed-sync-gtk` (which must match its
  `Cargo.toml` version). See `docs/linux-packaging.md` there.
