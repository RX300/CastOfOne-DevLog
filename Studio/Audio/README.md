# Audio department

Responsible for sound effects and music. Available: WeLoveIndies (free for noncommercial use), fal audio models.

## Status

First demo batch complete (11/12, Claude): 16 sound effects + 1 music track, all generated with ElevenLabs on fal and imported into UE. Prompts are in `SourceAssets/Audio/PROMPTS.md`. Codex R2 handles integration (the brief lists the event corresponding to each sound).

## Inventory

| Sound | Asset | Use | Source/tool | Status |
|---|---|---|---|---|
| Clapper closing | `S_Clapper` | Clapper close (the most important sound in the demo) | ElevenLabs SFX v3 | Imported |
| Editing-state loop | `S_EditLoop_Projector` (loop) | Projector gears in editing state | ElevenLabs SFX v3 | Imported |
| Forgetting | `S_Forget_Shatter` | Glass breaking + tape tearing | ElevenLabs SFX v3 | Imported |
| Match Cut | `S_MatchCut_Chord` | Rising chord on alignment snap | ElevenLabs SFX v3 | Imported |
| Light / heavy slash | `S_Slash_Light` / `S_Slash_Heavy` | Flash cut | ElevenLabs SFX v3 | Imported |
| Hit | `S_Hit` | Flash-cut hit | ElevenLabs SFX v3 | Imported |
| Train | `S_Train_Pass` | Whistle + rushing past | ElevenLabs SFX v3 | Imported |
| Heavy slam | `S_Heavy_Slam` | Armored enemy's ground shockwave | ElevenLabs SFX v3 | Imported |
| Noise bite | `S_Noise_Bite` | White-noise attack | ElevenLabs SFX v3 | Imported |
| Door smash | `S_Door_Smash` | Train smashes the PLOT HOLE door | ElevenLabs SFX v3 | Imported |
| Typewriter | `S_Typewriter_Key` | Screenplay cards, character by character | ElevenLabs SFX v3 | Imported |
| NG | `S_NG_Scratch` | NG slate | ElevenLabs SFX v3 | Imported |
| Weightlessness | `S_Space_Float` | Entering deep-space area | ElevenLabs SFX v3 | Imported |
| Dodge / slow motion | `S_Dodge` / `S_SlowMo` | Dodge, perfect dodge | ElevenLabs SFX v3 | Imported |
| Main music | `M_Main_Heist` (90 s loop) | 1970s heist funk × spaghetti-western whistle with a little synth | ElevenLabs Music v2.5 | Imported |

Paths: `/Game/MegaJam/Audio/SFX/`, `/Game/MegaJam/Audio/Music/`. Source files: `SourceAssets/Audio/`.

## Next steps

- Low-pass the music in editing state (Codex R2 integration).
- Optional: an ambience layer for each layer (soundstage hum / western wind and sand / deep-space bass), music variations across three layers, crossfades on either side of cut lines (GDD §9).
- Optional: off-screen “Cut! Another take!” (NG), crew applause (Perfect Match Cut).

## Requests (from other departments)

-
