         
# seed-sync-binaries
![SEED Sync Header Image](seed-header.png)
Public **release artifacts** for **S.E.E.D. (Seed Sync)** — the Linux tarball and
the Windows installer. This repo holds binaries only; the source lives in the
source repo.

## How it works
- Each release is a **GitHub Release** tagged `vX.Y.Z`, carrying both platforms'
  assets:
  - `seed-sync-<ver>-linux-x86_64.tar.gz` — published automatically by the
    `seed-sync-gtk` **release workflow** on a `v*` tag.
  - the Windows installer (MSI/zip) — attached to the same release by the Windows side.
- It's **public on purpose**: the install/update tooling downloads assets with no
  authentication. Publishing *to* this repo requires a token (`SEED_BINARIES_TOKEN`,
  stored as a secret in source repo); downloading does not.

## Install / update / uninstall (Linux)

Per-user, no root. Requires **GTK 4.10+, libadwaita 1.4+, libdbus-1** on the system.

**One command** — installs, updates, or removes (detects what's there and prompts):

```sh
curl -fsSL https://steeb-k.github.io/seed-install.sh | sh
```

Non-interactive: append `-s -- install` (or `update` / `remove`) after `sh`.

After the first install, manage it locally with the `seed-sync` command:

```sh
seed-sync --update          # check + apply a newer release (a daily timer does this too)
seed-sync --status          # installed/latest version + service state
seed-sync --uninstall       # add --purge to also delete ~/.local/share/seedsync data
```

<details><summary>Manual install without the bootstrap</summary>

Per-user, no root. Requires **GTK 4.10+, libadwaita 1.4+, libdbus-1** on the system.

**Install** (also upgrades an existing install in place) — fetches the latest release:

```sh
cd "$(mktemp -d)" && curl -fsSL "$(curl -fsSL https://api.github.com/repos/steeb-k/seed-sync-binaries/releases/latest | grep -oE 'https://[^"]+linux-x86_64\.tar\.gz' | head -1)" | tar xz && seed-sync-*/install.sh
```

**Update** an existing install (a daily `systemd --user` timer already does this for you):

```sh
seed-sync-update            # add --check to only report, don't change anything
```

**Uninstall** (add `--purge` to also delete `~/.local/share/seedsync` data):

```sh
cd "$(mktemp -d)" && curl -fsSL "$(curl -fsSL https://api.github.com/repos/steeb-k/seed-sync-binaries/releases/latest | grep -oE 'https://[^"]+linux-x86_64\.tar\.gz' | head -1)" | tar xz && seed-sync-*/uninstall.sh
```

The installer drops binaries in `~/.local/bin`, runs the daemon as a `systemd --user`
service (auto-starts at login), and enables daily auto-updates. Launch **S.E.E.D.**
from your app menu. Flags: `install.sh --no-auto-update`, `install.sh --no-gui-autostart`.

</details>

**Windows:** download the installer asset from the latest release and run it.

## Maintainers
- **Do not commit source or large files here** — only release assets, attached via
  Releases (not committed to the tree).
- Releases are cut by tagging `vX.Y.Z` in `seed-sync-gtk` (which must match its
  `Cargo.toml` version). See `docs/linux-packaging.md` there.
