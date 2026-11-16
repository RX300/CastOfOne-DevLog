# Release department

Responsible for packaging, video, dev log and itch submission. Requirements are in `JAM_RULES.md`.

## Status

2026-11-16: Curated English Dev Log prepared in `DEV_LOG/` (14 documents plus index, 31 image files, department hierarchy retained). Dedicated Public repository https://github.com/RX300/CastOfOne-DevLog created and pushed; Public visibility and all 46 uploaded files verified. Unselected material is in `MegaJamProject/Saved/DevLogTranslationWork/`.

2026-11-16: Windows x64 Butler 15.32.0 downloaded to `Tools/butler/`; `version` runs successfully. Not logged in or uploaded. Game packaging and submission still pending.

2026-11-16 08:26: **Windows build packaged and launch-tested**: folder `Build/CastOfOne_Win64/` (4.1 GB, double-click `CastOfOne.exe`), ZIP `Build/RX300_CastOfOne.zip` (4.0 GB). No Butler login or upload yet. See Packaging below.

2026-11-16 09:15: Trailer **recut** from the producer's OBS gameplay recording: `Trailer/CastOfOne_Trailer.mp4` (`Trailer/CastOfOne_Trailer.mp4`; project reference, not included), 59.6 seconds, 1920×1080, 60 fps, **not uploaded to any video site**; submission needs a link. Game title **CAST OF ONE** (decided 07:30).

## Submission checklist

- [x] Complete first test package on D2 (Windows) and actually run it (11/16 08:26; launch smoke tests only, no manual packaged-build playtest)
- [ ] Final package, ZIP named `TeamName_GameName.zip`; use Butler above browser-upload limit (package ready: `Build/RX300_CastOfOne.zip`, **rename for the team**; 4 GB exceeds browser upload limit, use Butler)
- [ ] Upload to own itch.io account
- [x] 30–60-second gameplay video, gameplay within first 5 seconds (file ready, see Trailer; uploaded link pending)
- [x] Curated English Dev Log prepared (`DEV_LOG/`; GitHub publication verified)
- [ ] Team name, member names + Epic account emails, game title
- [ ] Tools + third-party assets (`CREDITS.md`)
- [ ] Pre-jam preparation declaration (`PRE_JAM_DECLARATION.md`)
- [ ] Submit before deadline (11/16 10:59 JST)

## Next steps

Complete Windows packaging and playtest; create itch.io game page, log into Butler, upload to the corresponding project, then submit to the jam.

## Butler upload

- Official stable download: [Windows x64 ZIP](https://broth.itch.zone/butler/windows-amd64/LATEST/archive/default). Local files include `butler.exe`, `7z.dll`, `c7zip.dll` and original ZIP.
- [Official installation instructions](https://itch.io/docs/butler/installing.html) · [Upload instructions](https://itch.io/docs/butler/pushing.html). Checked 2026-11-16: supports folders or ZIPs, up to 30 GB uncompressed total.
- Run the following in PowerShell; replace path, username and project name in the upload command with actual values:

```powershell
& 'F:\UnrealLib\Projects\MegaJam\Tools\butler\butler.exe' login
& 'F:\UnrealLib\Projects\MegaJam\Tools\butler\butler.exe' push 'F:\UnrealLib\Projects\MegaJam\Build\CastOfOne_Win64' 'username/project:windows'
```

`login` opens browser authorization. Upload the packaged directory containing the game EXE and runtime files, not the UE project directory; no need to ZIP first. Subsequent uploads to the same `windows` channel update that version. After uploading to the game page, still submit the game on the jam page.

## Packaging

- **Deliverables**: `Build/CastOfOne_Win64/` (59 files, 4.1 GB) and `Build/RX300_CastOfOne.zip` (same content, stored without further compression, top-level `CastOfOne/`). `Build/_fallback_fullres_textures_4.8GB/` is the first version without texture caps; **do not upload**, retained only as backup.
- **Configuration**: Win64 Development (not Shipping), IoStore + Oodle compression, no debug symbols, VC runtime installer or crash reporter, English only. Development allows the `~` console and cheat commands; engine on-screen debug messages are disabled with `bEnableOnScreenDebugMessages=False` (`DefaultEngine.ini`, **also disables them in the editor**).
- **Command** (editor must be closed; see Pitfalls):

```bash
"F:/UnrealLib/UE_5.9/Engine/Build/BatchFiles/RunUAT.bat" BuildCookRun -project="F:/UnrealLib/Projects/MegaJam/MegaJamProject/MegaJamProject.uproject" -noP4 -platform=Win64 -clientconfig=Development -build -nocompileeditor -skipbuildeditor -cook "-AdditionalCookerOptions=-ini:Engine:[DevOptions.Shaders]:NumUnusedShaderCompilingThreads=9" -stage -pak -iostore -compressed -nodebuginfo -archive -archivedirectory="F:/UnrealLib/Projects/MegaJam/Build/Small" -utf8output -unattended
```

  Output is at `<archivedirectory>/Windows/`. I renamed it `CastOfOne_Win64`, renamed the root launcher `CastOfOne.exe`, and deleted `Manifest_*.txt`. First full Cook took about 12 minutes; subsequent incremental Cook about 2.5 minutes.
- **Verification**: `python Tools/package_smoke.py Build/CastOfOne_Win64` (EXE name in script must match): directly launched main menu and each of the three maps for 25–55 seconds; no assertions, crashes or missing assets in logs. **Only verified launch and running**: did not enter levels through the packaged main menu, playtest, or inspect visuals.
- **Size reduction** (4.8 → 4.1 GB): `DefaultDeviceProfiles.ini` sets `MaxLODSize=2048` for Windows world / character / weapon / vehicle texture groups, discarding mips above 2K during cooking (many Fab packs contain uncapped 4K textures), leaving sources unchanged. Of 3.4 GB compressed data, textures are about 2.5 GB, meshes 0.7 GB, shaders 0.14 GB. Reducing normals/specular to 1K would save only another 0.4 GB; producer said unnecessary.
- **Not done**: Shipping configuration (could save about 200 MB in EXE and remove console, but this project has never built/tested Shipping); removing bundled path-tracing denoiser models (about 90 MB; marking `DirectoriesToNeverCook` causes Cook failure, so plugin must be disabled).
- **Pitfalls**: ① With editor open, Cook exhausts system commit memory and shader compiler processes OOM (first attempt failed this way); passed after closing editor and limiting shader concurrency to 7. ② Assets loaded by path with `LoadObject` do not automatically enter the package; first build asserted on main-menu launch because background textures were missing. `DefaultGame.ini` now has `DirectoriesToAlwaysCook` entries for `Mechanisms/Interact`, `Mechanisms/Levers`, `UI/Fonts`, `UI/Menu`, `Stage03/Materials`, `/Engine/BasicShapes`. When adding path-loaded assets in code, add their directories.

## Trailer

- Final `Trailer/CastOfOne_Trailer.mp4` (`Trailer/CastOfOne_Trailer.mp4`; project reference, not included): 59.6 seconds, 1920×1080, 60 fps, about 117 MB (recut 11/16 09:15, puppet section added 09:20). `_draft.mp4` in the same directory is the 540p draft. **Not uploaded to a video site.**
- Source: producer's own OBS playthrough of three stages (`D:/Obs Videos/2026-11-16 09-00-47.mp4`, copy at `MegaJamProject/Saved/Trailer/raw/obs_playthrough.mp4`). OBS recorded the game at the upper left of a 1920×1080 canvas with black borders elsewhere; edit crops the 1504×846 game picture and scales it to 1080p.
- Structure: 0–17.35 seconds is **continuous gameplay** (speed changes only, no jump cuts): approach plate → stand on it to open gate, step off to close → press `R` while on plate to record → stop and leave plate → `TAB` opens desk, drag film onto timeline → back to set, `G` rolls, double stands on plate (this half-second slowed to 0.5×) → player runs through gate. Then A LEG UP → A HAND-OFF (double passes key) → Stage 02 (leave cabin, snow valley, wade through snow to find key, unlock; one continuous take at 1.2×) → Stage 03 (manor exterior into hall, library, dining room, costume-room corridor) → from 52 seconds DO NOT LOOK AWAY puppet mechanic in prop store (still while stared at, turn away/back and it is behind you; 4.6 seconds continuous gameplay) → title at 56.6 seconds, held 3 seconds.
- Rebuild: cards `python Tools/trailer/cards.py`, edit `python Tools/trailer/build.py` (add `draft` for draft), edit list is `SECTIONS` at top of script (source start/end seconds and speed). Old script-driven capture edit list is `Tools/trailer/archive/build_capture_cut.py`; that capture system (`Tools/ue_scripts/trailer_capture_*.py`) was not used this time. ffmpeg comes from Python `imageio_ffmpeg`.
- Music: fal.ai ElevenLabs Music v2.5, 60 seconds, `SourceAssets/Audio/Trailer/trailer_bgm_v1.mp3`.
- Known limitations: music only, no recorded game audio (game already has music, which conflicts with trailer music); I could not listen to the final cut, beat alignment checked only by bar timings; footage is upscaled from 1504×846, not native 1080p; game captions and HUD remain visible.

## Requests (from other departments)

-

## Change log

- 2026-11-16: Downloaded/verified Butler, added upload workflow for files over 1 GB.
- 2026-11-16 07:30: Named CAST OF ONE, 60-second trailer completed, added Trailer section.
- 2026-11-16 08:26: First successful package and smoke verification, added Packaging section.
- 2026-11-16 09:15: Trailer recut from producer's OBS recording; rewrote Trailer section. At 09:20, shortened title to 3 seconds and added puppet segment at producer's request.
