# Build information

The portable binaries attached to the GitHub release were built from EffectCraft commit
`608d1b077b65bb7591f26be587a2a5a47f4278bc` with Rust 1.95.0 for
`x86_64-pc-windows-msvc`.

The build includes the linked `org.effectcraft.text-box` plug-in (TextBox 1.1), signed padding,
geometric Matte, the single-row font search in Character and Properties, and native monitor-edge
cursor wrapping for numeric scrubs (8 October 2026).

Latest verification for the cursor-wrapping update:

- UI unit tests: 105 passed, including 8 cursor-wrapping tests.
- UI integration tests: 17 passed (angles, drag/drop, font search, text editing).
- Repeated wraps in both directions, vertical/corner wraps, offset monitor bounds, modifiers,
  release/focus loss, unsupported warp fallback, and real Properties/TextBox Undo were checked.
- Windows x64 portable GUI and CLI build: passed; static CRT enabled.
- Formatting: passed. Clippy completed with only the two existing warnings in effectcraft-effects.
- The native executable launched and responded to its control channel. Windows screen capture
  returned timeouts, so a physical-mouse end-to-end check was not completed.

Build command (PowerShell, from the patched repository):

```powershell
$env:CARGO_PROFILE_RELEASE_LTO = 'false'
$env:CARGO_PROFILE_RELEASE_CODEGEN_UNITS = '16'
$env:CARGO_TARGET_DIR = 'target\portable'
$env:CARGO_TARGET_X86_64_PC_WINDOWS_MSVC_RUSTFLAGS = '-C target-feature=+crt-static'
$env:EFFECTCRAFT_REQUIRE_WINRES = '1'
cargo +1.95.0 build --locked --release --target x86_64-pc-windows-msvc -j 6 -p effectcraft -p effectcraft-cli
```

Earlier verification for the included TextBox/font-search changes:

- `cargo test --offline --release -p effectcraft-text-box --lib`: 17 passed, 1 ignored.
- `cargo test --offline --release -p effectcraft-host --test text_box --test plugins`: 7 passed.
- `cargo test --offline --release -p effectcraft-ui-egui --test ui_font_search --test ui_text_edit`: 7 passed.
- `cargo build --offline --locked --release --target x86_64-pc-windows-msvc -p effectcraft -p effectcraft-cli`: passed.
- The demo composition was rendered through the packaged CLI in CPU and GPU modes.

The project source keeps the upstream MIT OR Apache-2.0 notices. This is a modified build and is
not an official EffectCraft release. The ArtCraft mark was removed from the modified executable
and replaced by the plain `EC` initials, as required for modified versions.

## Publication preparation (10 October 2026)

The Infinite Drag test conditionally checks TextBox when that optional plugin is installed;
this lets its patch run independently. Runtime feature code is unchanged from the portable build.
The source About text additionally names Infinite Drag. Upstream trademark artwork and screenshots
are excluded from this source snapshot, and upstream automation workflows are not enabled here.

Publication checks:

- Each feature patch applies independently to the pinned upstream base.
- All six application orders of the three patches produce the same source tree.
- Formatting, dependency layering and asset attribution checks passed (25 assets).
- The publication source passed all 105 UI unit tests, including the 8 pointer-wrap
  tests, and the 17 angle, drag/drop, font-search and text-editing integration tests.
- TextBox and host checks passed: 17 TextBox unit tests (1 manual benchmark ignored),
  14 host unit tests and 7 plugin/TextBox integration tests.
- An expanded `cargo test --release -p effectcraft-ui-egui --lib --tests` run
  stopped at `ui_gpu::viewer_draws_gpu_frames_that_match_the_cpu`:
  `crates/ui-egui/tests/ui_gpu.rs:41`, assertion `GPU compositor on egui-wgpu's device`.
  A headless NVIDIA Vulkan adapter was found, but the test viewer had no GPU
  compositor. The test and GPU implementation are unchanged from upstream;
  the root cause has not been established. Integration suites after `ui_gpu`
  were not run in that command; `ui_text_edit` was subsequently run and passed.
- The full `cargo xtask ci` gate stopped in unchanged upstream code at
  `crates/psd/src/lib.rs:660`: Clippy `collapsible_match`, promoted to an error by
  `-D warnings`. Later full-workspace CI stages did not run; this is not a claim
  of a green upstream CI run.
