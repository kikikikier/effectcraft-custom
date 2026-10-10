# EffectCraft Custom

A community build of [EffectCraft](https://github.com/storytold/effectcraft) combining
**Infinite Drag + Type-in Font Search + TextBox Plugin**.

This repository contains the complete source. It is an independent modification of EffectCraft
by the ArtCraft Team, not an official project release.

## Download for Windows

[Download the Windows x64 portable build](https://github.com/kikikikier/effectcraft-custom/releases/latest).
Extract the ZIP and run `effectcraft.exe`. TextBox is already included; no separate installation is needed.

## Features

| Standalone patch | What it adds |
|---|---|
| [Infinite Drag](https://github.com/kikikikier/effectcraft-infinite-drag) | Continuous numeric dragging across monitor edges, with one Undo step per gesture |
| [Type-in Font Search](https://github.com/kikikikier/effectcraft-type-in-font-search) | Search directly in the font selection field in Character and Properties |
| [TextBox Plugin](https://github.com/kikikikier/effectcraft-textbox-plugin) | Text backgrounds with negative padding and a geometric matte independent of fill opacity |

## Build from source

Based on upstream commit [`608d1b077b65bb7591f26be587a2a5a47f4278bc`](https://github.com/storytold/effectcraft/commit/608d1b077b65bb7591f26be587a2a5a47f4278bc).
Requires Git, Rust 1.95.0 and the C++ build tools for Windows (MSVC).

```powershell
git clone https://github.com/kikikikier/effectcraft-custom.git
Set-Location effectcraft-custom
cargo +1.95.0 build --locked --release -p effectcraft -p effectcraft-cli
```

See [CUSTOM-BUILD.md](CUSTOM-BUILD.md) for a portable build with a statically linked CRT.
The [patches](patches) directory contains the independent patches for a clean upstream checkout.
All changes are already integrated here; do not apply the patches to this repository again.

## Validation and limitations

The Windows build and 122 UI tests passed during portable-build preparation, including 8 cursor-wrap
tests. TextBox was also checked through the host/compositor and CPU/GPU rendering.
The physical-mouse end-to-end check was not completed because Windows screen capture timed out.

Publication checks repeated all 105 UI unit tests and the 17 angle, drag/drop, font-search and text-editing
tests successfully. TextBox and host checks passed 38 tests, with one manual benchmark ignored.
Full upstream CI is not green: Clippy stopped on a warning in unchanged upstream code. An expanded
UI run stopped at `ui_gpu` because the test viewer did not initialize the GPU compositor; the root
cause has not been established. See [CUSTOM-BUILD.md](CUSTOM-BUILD.md) for details.

## Credits and license

**MIT OR Apache-2.0**; see [LICENSE-MIT](LICENSE-MIT), [LICENSE-APACHE](LICENSE-APACHE)
and [NOTICE](NOTICE). Original EffectCraft attribution is preserved.
The original TextBox plugin was provided by kikikikier; this build includes the updated version.
ArtCraft trademark artwork is replaced by plain EC initials in the UI and excluded from the
published documentation. Openly licensed assets are listed in [ATTRIBUTION.md](ATTRIBUTION.md).
