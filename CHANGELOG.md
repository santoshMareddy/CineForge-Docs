# CineForge Changelog

## 2.0.0 — 2026-07-03

First public release. Complete rewrite unifying three previous internal plugins
(LumenOneClick / CinematicHQ, MRQOneClick, CinematicCameraPresets) into one.

**Compile-verified on UE 5.4.4, 5.5.4, 5.6.1, 5.7.4 and 5.8.0 — prebuilt Win64
binaries ship for all five.** (On UE 5.4, the camera tool's Add-to-Sequencer
option is unavailable due to an engine header issue; everything else is
identical across versions.)

### Added
- **Unified plugin**: one toolbar menu + dockable Control Panel (Window → CineForge).
- **⚡ Lumen HQ**: Cinematic / Pro / Simple presets, Scene Validator (with real
  RHI/ray-tracing detection), safe cancellable Nanite enabling (current level by
  default, whole project opt-in), persistent settings written to project config,
  smart DX12 + SM6 + Ray Tracing setup with a guided editor-restart flow.
- **🎯 Path Traced preset**: reference path tracer mode with automatic viewport
  view-mode switching and laptop-safe defaults (8 bounces / 512 spp).
- **❌ Disable All**: one click returns project settings, PostProcessVolumes,
  console variables and the viewport to engine defaults — and removes every
  config entry the plugin ever wrote.
- **🎬 Movie Render Queue**: Cinematic 4K / Balanced 4K / Preview 1080p presets,
  **Render Now** (attaches to the open Level Sequence and starts the render),
  preset saving to `/Game/CineForge/MRQ`, multilayer EXR with AOV compositing
  passes, tiered render-time Console Variables (Max = 14 CVars, Few = 5, None).
- **🎨 ACES export**: one toggle switches the project Working Color Space to
  ACEScg and enables an OpenColorIO transform (ACEScg → ACES2065-1) on the MRQ
  Color Output using UE's built-in ACES config — EXRs become standard ACES
  interchange files. Automatic fallback to linear ACEScg if OCIO is unavailable.
- **📷 Cinematic Cameras**: six real lens presets (14mm f/8 → 200mm f/4) on a
  Full Frame sensor with auto depth of field, actor-tracking focus, aspect-ratio
  presets (Full Frame, 16:9, 1.85:1, 2.39:1 anamorphic, 1:1), optional
  add-to-Sequencer, and viewport pilot on spawn.

### Fixed (vs. the v1 plugins)
- Fatal crash entering Path Tracing view mode in sessions where the path tracer
  shaders were not compiled at startup.
- GPU driver timeout (TDR) crashes from excessive path-tracing bounce counts.
- Project settings silently never persisting (SaveConfig vs default config file).
- Ray-Traced AO stacked on Lumen causing double-darkened corners and edges.
- Lumen short-range AO / reflection screen-trace edge artifacts ("dark edges").
- Ray-traced shadows fighting Nanite fallback meshes.
- Editor hangs from force-loading every static mesh during Nanite enabling.
- Camera spawn failures on repeated spawns of the same lens preset.
- Cross-version guards that would break on future engine major versions.
