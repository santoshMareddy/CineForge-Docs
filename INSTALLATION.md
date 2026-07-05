# CineForge v2.0.0 — Installation Notes

CineForge is an **editor plugin** for Unreal Engine **5.4 → 5.8** (Windows, DirectX 12).
Pick the package for your engine version:

| Package | Engine | Compiler needed? |
|---|---|---|
| `CineForge-v2.0.0-Win64-UE5.4.zip` | **UE 5.4** | No — copy & use |
| `CineForge-v2.0.0-Win64-UE5.5.zip` | **UE 5.5** | No — copy & use |
| `CineForge-v2.0.0-Win64-UE5.6.zip` | **UE 5.6** | No — copy & use |
| `CineForge-v2.0.0-Win64-UE5.7.zip` | **UE 5.7** | No — copy & use |
| `CineForge-v2.0.0-Win64-UE5.8.zip` | **UE 5.8** | No — copy & use |
| `CineForge-v2.0.0-Source.zip` | future 5.x releases | Yes — one-time rebuild (see below) |

> Prebuilt binaries only load on the exact engine version they were built for.
> On UE 5.4, the camera tool's *Add to Sequencer* option is unavailable (engine
> limitation); everything else works identically.

---

## Option A — Install into one project (recommended)

1. Close the Unreal Editor.
2. In your project folder (where `YourProject.uproject` lives), create a `Plugins`
   folder if it doesn't exist.
3. Unzip the package so you get:
   ```
   YourProject/
   └── Plugins/
       └── CineForge/
           ├── CineForge.uplugin
           ├── Source/ ...
           └── (Binaries/ if using the Win64 package)
   ```
4. Open the project.
   - **Win64 package on UE 5.7:** it just loads.
   - **Source package:** the editor asks *"Missing CineForge modules — rebuild now?"*
     → click **Yes** (requires Visual Studio 2022 with the *Game development with C++*
     workload; the build takes 1–3 minutes, one time only).
5. Done. Look for **CineForge** on the Level Editor toolbar (next to the Play
   controls), or open **Window → CineForge** for the control panel.

## Option B — Install engine-wide (all projects)

> ⚠️ Only do this with binaries that **exactly match** your engine version
> (e.g. the Win64-UE5.7 package on UE 5.7). Engine-installed plugins can NOT be
> rebuilt from the editor — a version mismatch here shows
> *"Engine modules cannot be compiled at runtime"* and refuses to load.
> When in doubt, use Option A (project install): it offers automatic rebuilding.

1. Close the Unreal Editor.
2. Unzip into your engine:
   ```
   C:\Program Files\Epic Games\UE_5.7\Engine\Plugins\Marketplace\CineForge\
   ```
   (Copying into Program Files may require Administrator.)
3. Enable it per project via **Edit → Plugins → Cinematics → CineForge** if it
   isn't enabled already.

---

## First run — 60-second quick start

1. **CineForge → ⚡ Lumen HQ → 🎬 Cinematic** — sets up Lumen, Nanite, VSM,
   a PostProcessVolume, and validates your scene.
   - If hardware ray tracing isn't active yet, CineForge configures
     **DX12 + SM6 + Ray Tracing** for you and offers a one-click editor restart.
     Accept it (first launch recompiles shaders once).
2. Open a Level Sequence, then **CineForge → 🎬 Movie Render Queue →
   ▶ Render Cinematic 4K** — renders it immediately as multilayer EXR.
3. Want ACES? Tick **🎨 ACES Export (ACEScg)** first — EXRs come out as
   ACES2065-1 interchange files that Resolve/Nuke read directly.
4. **📷 Cinematic Camera → 85mm Beauty** — spawns a Full Frame CineCamera with
   real DOF at your viewport position (select an actor first to auto-track it).
5. Everything with extra toggles lives in the panel: **Window → CineForge**.

## Requirements

- **Windows 10/11**, DirectX 12
- **Simple preset** (software Lumen): any DX12 GPU
- **Cinematic / Pro / Path Traced**: ray-tracing GPU (NVIDIA RTX / AMD RX 6000+)
- **Source package builds**: Visual Studio 2022, *Game development with C++* workload
- MegaLights requires UE 5.5+ (handled automatically on 5.4)

## Don't have Visual Studio? (no-compile options)

You only ever need to compile if your package's binaries don't match your engine
version. In order of convenience:

1. **Install from Fab / Epic Games Launcher** (when available): Epic ships the
   plugin prebuilt for every supported engine version — click *Install to
   Engine* and you're done. No compiler, ever.
2. **Use a prebuilt zip that matches your engine** — e.g.
   `CineForge-v2.0.0-Win64-UE5.7.zip` on UE 5.7 loads instantly, including in
   Blueprint-only projects.
3. **Install the free compiler once** — Visual Studio 2022 **Community** costs
   nothing: download from visualstudio.microsoft.com, select the
   **"Game development with C++"** workload during install, then reopen your
   project and click **Yes** on the *"rebuild CineForge modules?"* prompt.
   One-time setup (~15–30 min), after which the plugin builds itself in 1–3 min.

## Common questions

- **"Engine modules cannot be compiled at runtime. Please build through your
  IDE."** → two things at once: the binaries don't match your engine version
  (e.g. the UE 5.7 package on UE 5.5), AND the plugin sits in the **engine's**
  Plugins folder — UE only offers the automatic rebuild for **project** plugins.
  Fix: remove it from the engine folder, unzip the **Source** package into
  `YourProject\Plugins\CineForge\`, reopen the project, click **Yes** on the
  rebuild prompt. (Prebuilt binaries only ever work on the exact engine version
  they were built for.)
- **"Missing modules" prompt won't build** → your machine has no C++ toolchain;
  use a prebuilt package matching your engine version (options 1–2 above), or
  install VS 2022 Community (option 3).
- **Red "no ray tracing data" messages** → apply any RT preset and accept the
  restart prompt; CineForge fixes the project settings for you.
- **Path Tracing view won't activate** → accept the restart prompt after applying
  the Path Traced preset once (PT shaders compile at editor startup).
- **Laptop GPU driver resets during path tracing** → raise the Windows GPU
  watchdog (run as admin, then reboot):
  `reg add "HKLM\SYSTEM\CurrentControlSet\Control\GraphicsDrivers" /v TdrDelay /t REG_DWORD /d 60 /f`
- **Undo everything CineForge changed** → ⚡ Lumen HQ → **❌ Disable All**, then
  save when prompted. Every setting and config entry the plugin wrote is removed.

---

*CineForge 2.0.0 · Commercial (Fab EULA) · © 2024-2026 Happy*
