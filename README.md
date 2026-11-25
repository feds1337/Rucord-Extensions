# Rucord Extensions

Public community repository for Rucord themes and extensions. Packages are distributed as static files from GitHub; there is no account, backend, or automatic code execution service.

The Rucord app checks `catalog.json` when you open Themes or Extensions. Catalog entries pin each package and preview to an immutable commit and include their SHA-256 and byte length. Rucord verifies those values before installing anything. Installation and updates remain explicit and require user consent.

## Browse the catalog

Install Rucord from the [latest stable release](https://github.com/feds1337/Rucord/releases/latest), then open **Settings → Themes** or **Settings → Extensions**. The last valid catalog and installed packages remain available offline. If a package is no longer listed, Rucord does not remove it from your device.

## Repository layout

| Path | Contents |
| --- | --- |
| `themes/` | Declarative theme packages |
| `plugins/<id>/` | Plugin source and manifest |
| `plugins/packages/` | Reviewed packages that Rucord can install |
| `sdk/` | Rust SDK for native plugin handlers and Wasm exports |
| `previews/` | Licensed theme and plugin preview images |
| `licenses/` | Third-party license texts |
| `catalog.json` | Generated catalog; do not edit by hand |
| `catalog.py` | Catalog validator and pin generator |

## Submit a theme or extension

1. Read the [extension safety and review guide](https://github.com/feds1337/Rucord/blob/main/docs/extensions.md), [theme package format](https://github.com/feds1337/Rucord/blob/main/docs/theme-api.md), and [SDK authoring guide](https://github.com/feds1337/Rucord/blob/main/examples/extensions/README.md).
2. Add the package, source, required license and reproducible build instructions. Keep the original author and source attribution. Bump the package version when its bytes change.
3. Add an optional PNG preview under `previews/` using the package file stem. Use artwork you created or are licensed to redistribute.
4. Open a pull request. The catalog validator checks package bounds, unique IDs, source manifests, package hashes, licenses and preview metadata. Plugin source is checked with the pinned Rust toolchain.
5. Maintainers review the source and exact package bytes together. After merge, the catalog workflow pins packages and previews to the merged commit.

Validate locally before opening a pull request:

```sh
python3 -m unittest test_catalog.py
python3 catalog.py --validate
rustup target add wasm32-unknown-unknown
cargo check --locked --workspace --target wasm32-unknown-unknown
```

After a merge, the catalog can be regenerated locally if GitHub Actions is unavailable:

```sh
python3 catalog.py --pin "$(git rev-parse HEAD)"
```

## Attribution and licensing

Every package retains its manifest author, source link, and package-level license. Some entries are preserved imports from existing projects; the original creators and license terms remain credited. This repository does not claim authorship of imported themes, artwork, or plugin ideas, and its existence does not replace the license for any package.

See each package manifest and source directory for its individual attribution and license. For catalog or packaging questions, open an issue or pull request in this repository.
