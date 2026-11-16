# DLSS → UE 5.9 plugin port

v1 · 2026-11-16 · Codex. **Plugin-level integration of SR / DLAA, FG / MFG and Reflex is complete. Standalone DX12 testing in Stage 01 verified actual presented frame counts for 2X, 4X and 6X. RR has not demonstrated a clear benefit to the current visuals; no further RR work will be done this round and it remains off by default.**

## Technical boundaries and current configuration

- The producer's hard constraint: **do not modify engine source, rebuild or download the engine; only modify and compile plugins**. Met in this round. Builds use the existing Installed Build; subsequent incremental builds all use `-NoEngineChanges`. Shared precompiled headers used for building were generated inside the test project, not by rebuilding engine modules.
- Local engine: `F:/UnrealLib/UE_5.9`, 5.9.0 / CL 60351404. RTX 5060 Laptop, driver 595.79.
- Installed: `MegaJamProject/Plugins/NVIDIA/{DLSS,StreamlineNGXCommon,StreamlineCore,StreamlineReflex,StreamlineDLSSG}`. The `.uproject` explicitly enables all five plugins; module BuildIds match the engine (60351404).
- `[SystemSettings]` in `DefaultEngine.ini` enables `r.NGX.DLSS.Enable=1`, `r.NGX.DLSS.Preset=13` (M), `r.TemporalAA.Upscaler=1`, and `r.NGX.DLSS.DenoiserMode=0` (RR off). Disabled in ordinary editor viewports; can be enabled in PIE.
- `r.AntiAliasingMethod=4` is retained; the plugin takes over the existing temporal-upscaling entry point. Game C++, Blueprints, maps and gameplay were not modified; `AStageLook` still controls level input resolution. Stage 01 actually used 67% this time, not the 85% in older performance records.
- FG defaults to 2X: `r.Streamline.DLSSG.Enable=1`, `r.Streamline.DLSSG.FramesToGenerate=1`; Reflex uses normal low latency: `t.Streamline.Reflex.Enable=1`, `t.Streamline.Reflex.Mode=1`. MFG 4X / 6X were tested and can be selected through the plugin Blueprint API or console; no separate game settings menu was made. NIS, DeepDVC and MoviePipelineSupport are not installed.

## Originals, source and patch

The [official NVIDIA page](https://developer.nvidia.com/rtx/dlss) still listed support only through UE 5.8 on Nov 15. The local source is the official `2026.10.16_UE5.8_DLSS4.5Plugin_v8.9.0.zip`: this is a **local 5.9 adaptation, not an official NVIDIA 5.9 release**.

- Original package / unmodified extraction: `SourceAssets/Plugins/NVIDIA_DLSS/` and its `Original/` subfolder. Originals are unchanged.
- Versions: DLSS `8.9.0-NGX310.9.16`, bundled Streamline `2.14.16`. Original ZIP: 1,166,608,575 bytes, 1,104 entries, full extraction CRC passed; NVIDIA digital signatures on both Win64 production NGX DLLs are valid.
- ZIP SHA-256: `adeeb59b9635794e05252438bf01307bf8a629769e8f0517237363f690bb380c` (computed locally). Source, preparation baseline hashes and current state: manifest.json (`../../SourceAssets/Plugins/NVIDIA_DLSS/manifest.json`; project reference, not included).
- UE59.patch (`../../SourceAssets/Plugins/NVIDIA_DLSS/UE59.patch`; project reference, not included) records all source differences from the official plugins: `EngineVersion` changed to 5.9.0 in five `.uplugin` files; two DLSS source files and six StreamlineCore source / header files correct enum diagnostic arguments and integer format widths.
- UE 5.9's format-string checks reject the older plugins' enum / integer-width mismatches. Engine checks were not disabled, Renderer / RHI interfaces were not modified, and rendering algorithms were not rewritten.
- Editable isolated source: `Saved/DLSSPort/UE59/Plugins/`. Build output and assembled plugin packages: `Saved/DLSSPort/UE59/Packages/`, `Ported/`. Plugin source installed in the real project matches the patch.

## Building and reuse

Run from the repository root:

```powershell
& ./Tools/build_dlss.ps1
```

The build script (`../../Tools/build_dlss.ps1`; project reference, not included) reuses `Saved/DLSSPort/UE59/Packages/DLSS/HostProject`, enables all five plugins above, and builds `HostProjectEditor Development`, `HostProject Development`, then `HostProject Shipping`. It uses precompiled UBT and `-NoEngineChanges -NoUBA -NoUBALocal -MaxParallelActions=2`; it does not build the engine. Avoid launching a second UBT concurrently. Use ordinary project targets for the full dependency graph: building one plugin at a time with `-Plugin=` encounters Project / Plugin module hierarchy conflicts.

The first original `BuildPlugin` run with eight parallel jobs caused commit-memory pressure. The producer's idle editor was saved and closed, and building continued with two jobs. Logs with `NoUBA` still show the UBA scheduler, but compile actions are marked `[NoUba]`. Do not repeatedly run the original `BuildPlugin -Package=...` and clear the existing HostProject, as that discards reusable precompiled headers.

For installation, copy the isolated source and retain SDK DLLs in `Binaries/ThirdParty/`, libraries in `Source/ThirdParty/`, and Shaders / Content / Resources. `Binaries/Win64` comes from the local 5.9 HostProject build; do not copy UE5.8 modules from the original package. Previously `Ported/` contained only the SR / common-dependency two-plugin package; the real project installation and isolated source are authoritative for all five plugins. Deliver source, with Game targets compiled normally by the project; this is not claimed as a cross-project binary distribution.

| Verification | Result / evidence |
|---|---|
| Full five-plugin dependency graph, Editor / Development / Shipping | Passed: `Logs/Full-HostProjectEditor-Development.log`, `Full-HostProject-Development.log`, `Full-HostProject-Shipping.log` |
| MegaJamProject Editor / Game Development after FG/MFG addition | Passed: `Logs/Full-MegaJamProjectEditor-Development.log`, `Full-MegaJamProject-Development.log` |
| Stage 01 standalone DX12 FG / MFG / Reflex | Passed, actual presented frame counts 2 / 4 / 6; `FG-MFG.json`, `Logs/FG-MFG.log`, process exit=0, reason=ok, errors=0; screenshot [MFG](shots/DLSS_UE59_MFG.png) |
| Common dependencies, three targets | Passed: `Logs/Common-build.log` |
| DLSS plugin Editor / Development / Shipping | Passed: `Logs/DLSS-UnrealEditor-Development.log`, `DLSS-UnrealGame-Development.log`, `DLSS-UnrealGame-Shipping.log` |
| MegaJamProject Editor / Game Development | Passed: `Logs/Project-Editor.log`, `Project-Game.log` |
| Stage 01 standalone DX12 | Passed; SR, disabling back to TSR, DLAA and back to SR all ran; exit code 0 / reason=ok / errors=0; `Logs/Standalone.log`, `Standalone-result.json` |
| Earlier minimal SR Development packaging fixture | Cook / Stage / Archive passed; SR / Preset M feature creation succeeded inside the actual package, process exit code 0 / reason=ok / errors=0; `SmokePackaged/Windows/`, logs `Logs/Smoke-package.log`, `Packaged.log`, `Packaged-result.json`; not repackaged with FG/MFG |
| Earlier SR cold-start PIE | Stage 01 passed, `supported=true`, `enabled=true`, `rr_enabled=false`; log creates an `858×484 → 1280×722` Preset M / SR feature. Query record `PIE.json`, excerpt `Logs/PIE-DLSS.log`; FG/MFG were verified in standalone this round |

Relative paths in the table start at `Saved/DLSSPort/UE59/`. The minimal fixture's first Cook failed because disabling Cascade removed two `fx.GPUSimulationTextureSize*` variables. Built-in Cascade was re-enabled in the fixture; the original log `Smoke-package-initial.log` is retained. This did not modify the game project or engine source. After correction, Cook reported 0 errors / 4 warnings.

## Runtime evidence and limitations

Standalone game output was 1920×1080; the logs explicitly created:

- SR: `SrcRect=1287×724 → 1920×1080`, `Preset M(13)`, `MaxQuality(2)`, `DenoiseMode=Off`.
- DLAA: `1920×1080 → 1920×1080`, `Preset M(13)`, `DLAA(5)`.
- NGX support queries succeeded; actual input scale fell within the SDK's allowed range. Tests used `-ngxdisableota` to pin the bundled model; the project retains NVIDIA's default OTA settings.

The original game framebuffer, including HUD, was inspected: [DLSS SR](shots/DLSS_UE59_SR.png). A SceneCapture rendering another frame was not used to prove a temporal algorithm. Standalone execution emitted one `M_FabPBR` texture sampler-type warning and default-material replacement; similar warnings were already in editor logs before integration. That asset was not fixed this round. Cold-start editor logs also contain 13 `LogAutomationTest: Condition failed` entries; the pre-integration 23:14 backup log has the same 13. They were not classified as new DLSS errors, and engine tests were not changed.

Fixed entrance viewpoint in Stage 01, editor closed, 300 frames per group. No input or movement through the whole stage, so results cannot be extrapolated to full-level performance or motion ghosting quality:

| Mode | Median / P95 frame time (ms) | Median GPU time (ms) |
|---|---|---|
| TSR 67% | 14.64 / 16.76 | 13.82 |
| DLSS SR 67% / M | 15.02 / 16.63 | 14.11 |

**No clear speedup was measured here**; this does not claim DLSS is necessarily faster than the current TSR. These are usability checks. Raw CSVs: `MegaJamProject/Saved/Profiling/CSV/DLSS_UE59_*.csv`; summary: `Saved/DLSSPort/UE59/performance.json`.

Temporary console switching: `r.NGX.DLSS.Enable 0` returns to the original temporal upscaler; `1` enables DLSS. With DLSS enabled, `r.ScreenPercentage 100` means DLAA; Stage 01's `67` is the SR setting tested here. `StageLook` resets the scale at scene startup, so execute temporary commands after play starts. Use `UDLSSLibrary::EnableDLSS` / `GetDLSSModeInformation`; do not add calls to deprecated `SetDLSSMode` / `EnableDLAA`.

## Using FG / MFG

- Blueprint `Get Supported DLSS-FG Modes` queries modes available on the current machine, then `Set DLSS-FG Mode` selects `2X` / `4X` / `6X` or another returned mode. The C++ equivalent is `UStreamlineLibraryDLSSG`; NVIDIA's original API is retained.
- Console: for fixed generation first set `r.Streamline.DLSSG.Enable 1`; `r.Streamline.DLSSG.FramesToGenerate 1 / 3 / 5` means 2X / 4X / 6X. `Enable 0` disables it; restore defaults with `FramesToGenerate 1`, `Enable 1`. Higher multipliers require hardware support.
- **The standalone game window must be in the foreground.** In the editor, use New Editor Window (PIE); the ordinary embedded viewport does not support FG. NVIDIA's plugin disables generation in the background, and this behavior is retained. Initial reads showed 1 frame; after the producer brought it forward, actual reads showed 2 / 4 / 6.
- The criterion is `sl::DLSSGState::numFramesActuallyPresented` → `STAT_DLSSGFramesPresented`, not merely switches or game-thread FPS. `FG-MFG.json` is a single-frame state sample, not a benchmark of sustained frame-rate multipliers or input latency. Reflex SDK returned lowLatencyAvailable=1 and worked normally with FG; end-to-end input latency was not measured. This machine's API lists Off, Dynamic and 2X through 6X, but not Auto; therefore the default is tested, API-supported 2X. Dynamic MFG was not verified separately.
- Runtime logs: 0 errors, 37 warnings, including SDK handling of optional backbuffer extent / resource alignment and existing default-material replacement. Default Streamline OTA loaded version 2.14.16 from the local NVIDIA cache; `-ngxdisableota` pins only the NGX model used for this round's SR tests, and is not claimed to disable all Streamline OTA.

## Nov 16 RR usefulness assessment and scope

- The producer explicitly requested FG and MFG this round; RR should continue only if useful to the current game. Work then focused on `StreamlineCore`, `StreamlineReflex`, `StreamlineDLSSG`, without extending game functionality for NIS, DeepDVC or MoviePipelineSupport.
- Configuration and live editor reads: `r.RayTracing=1`, `r.Lumen.HardwareRayTracing=1`, `r.Lumen.Reflections.Allow=1`, LightingMode=0, ray-traced shadows off. 5.9 source explains that LightingMode=0 evaluates lighting through Surface Cache after ray hits. This does not mean there is no hardware ray tracing, nor does it mean fully path-traced GI. The older individual `r.Lumen.Reflections.HardwareRayTracing` / `r.Lumen.ScreenProbeGather.HardwareRayTracing` variables do not exist in this version; a failed read returning 0 must not be interpreted as disabled.
- A temporary standalone game followed the plugin's `EnableDLSSRR` procedure, disabling old reflection filtering / shadow denoising and setting DenoiserMode=1. NGX actually created `DenoiseMode=DLSSRR`; model logs showed RR Preset F, an image was visible, and exit=0 / errors=0. Screenshot [RR](shots/DLSS_UE59_RR.png), log `Saved/DLSSPort/UE59/Logs/RR-Assessment.log`. Project default RR configuration was unchanged.
- **Engineering recommendation: do not invest more in RR now.** Potential gains are primarily reflection noise / stability; it will not turn Surface Cache GI into full path-traced lighting. There is no evidence of a clear current-scene image-quality or frame-rate improvement. RR shares its plugin with SR; leave it disabled rather than removing code.
- Starting a temporary PIE test reproduced the documented `AStageLook::Tick` controller CastChecked crash before RR had been enabled. Content was saved first; rendering verification then used a standalone process with the project's GameMode explicitly selected. This crash is not attributed to RR. The initialization issue is outside this plugin-porting round.

## Follow-up and rollback

- The producer should next playtest motion quality, especially rewind, camera cuts, translucent doubles, first-person carrying arms and the editing desk. Continuous completion of all three stages, full-game Cook / packaging, and acceptance on other GPUs or DX11 / Vulkan are not complete.
- FG / MFG ran in standalone Stage 01 this round, but were not tested in a cooked package, a complete game package, Dynamic MFG or across GPUs. Cook normally for the current project before release. Game build receipts already include runtime dependencies `sl.dlss_g.dll`, `sl.reflex.dll`, `sl.pcl.dll`, `nvngx_dlssg.dll`. Engine changes remain prohibited for all follow-up work.
- Roll back FG/MFG: close the editor, move out the added `StreamlineCore` / `StreamlineReflex` / `StreamlineDLSSG` plugins, and remove the three corresponding enable entries and FG / Reflex ini entries. Pre-extension backups are in `BeforeFullIntegration/`. For a full rollback, move out all five plugins and remove their five enable entries and added DLSS ini entries; pre-integration backups are in `BeforeIntegration/`. Both are under `Saved/DLSSPort/UE59/`; do not overwrite other people's later changes.
- Bundled licenses: `Original/Documentation/NVIDIA RTX SDK EULA.pdf`, `Original/Plugins/DLSS/Source/ThirdParty/NGX/LICENSE.txt`. Guides: `Original/Documentation/DLSS_Super_Resolution_UE_Plugin_Guide.pdf`, pp. 6–8, 17–18, and `Original/ReleaseNotes_DLSS_Streamline_UE_Plugins.pdf`, pp. 1–2.

Change log: Nov 15, downloaded and prepared; the producer authorized starting the same day. Plugin-side format corrections, three-target builds and project integration completed; standalone SR/DLAA, minimal cooked package and cold-start PIE checks passed. Editor restored to Stage 01, outside PIE. No engine modification / rebuild at any point.

Nov 16: completed FG / MFG / Reflex; all five plugins and project builds passed; actual 2X / 4X / 6X verified in Stage 01. RR was evaluated by usefulness to the current game and remains off, with no further investment.

At handoff, the Stage 01 editor was restored with `IsPIERunning=false`; default readback was `ON2X`, support queries true, Reflex=`Enabled`, RR=0.
