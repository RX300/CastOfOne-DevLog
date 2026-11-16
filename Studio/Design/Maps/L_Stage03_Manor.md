# Level Three · L_Stage03_Manor (Twilight Mansion Set)

> **Selected translated excerpts; full historical specification is not included.**
> Source: `Studio/Design/Maps/L_Stage03_Manor.md`, v1, 2026-11-15, including its later 11/16 exit-condition note.
> Suggested UE level asset: `/Game/MegaJam/Maps/L_Stage03_Manor`.
> Related: [Mechanics](../MECHANICS.md), Softlock Rules (`../SOFTLOCK_RULES.md`; project reference, not included), Level Design Rules (`../LEVEL_DESIGN_RULES.md`; project reference, not included).

## Concept and Progression

At twilight, a house used to shoot horror films. In the front house, players explore, read production paperwork, and solve two small puzzles. Beyond wardrobe storage, the supposedly motionless dummies begin walking whenever nobody watches them.

The producer requested an above-ground, two-floor mansion with sunlight entering the rooms, gradual enemy introduction, approximately four increasingly difficult puppet puzzles, a windowed long gallery with visible light shafts, stamina, and resistance to repeatedly replayed doubles. The design implements two enemy-free warm-ups, `S1` and `S2`, followed by puppet puzzles `P1`–`P4`. The earlier elevator, underground passage, and cable proposals are absent. The level ends in the second-floor screening room; the previously discussed “other-side setting” is outside its scope.

The front foyer, dining room, and library have no enemies. Three early documents mention misplaced dummies without calling them alive or monsters. Introduction then proceeds from static tagged figures, to a living figure safely confined behind glass, to watching-only encounters, to receiving guns and killing instructions, and finally to crossfire. All living puppets are figures the player has already seen or passed; they do not materialize in unseen corners. Safe rooms separate the major encounters.

| Section | Intended Experience | First-Visit Estimate |
|---|---|---|
| Front house | Exploration, two warm-ups, three documents | 5–6 minutes |
| Wardrobe storage | Walk close past dummies; glass-case demonstration | 1.5 minutes |
| `P1` fitting room | One puppet; manage sight and occlusion | 3 minutes |
| Workshop | Safety; observe `M2` through glass | 0.5 minutes |
| `P2` prop store | One puppet, trolley, double as watcher | 4 minutes |
| Rear stairwell / gun cage | Safety; obtain gun and instructions | 1 minute |
| `P3` rehearsal hall | First puppet shattered by crossfire | 3 minutes |
| `P4` west gallery | Two-puppet finale | 6 minutes |
| Study / upper gallery / screening room | Safety and conclusion | 1 minute |

The design estimates about 25 minutes in total. This is an intended first-visit estimate, not an observed complete playthrough.

## Spatial Layout and Drawings

Design coordinates are meters: `+X` east, `+Y` north, `+Z` up. Ground floor is `Z = 0`; second floor is `Z = +5.0`. The front entrance faces south. The sun is west-southwest, approximately azimuth `250°`, elevation `9°`, casting shallow rays toward east-northeast. West windows receive direct light; south windows produce diagonal, elongated shafts.

The building is L-shaped: a horizontal main house to the south, a west wing extending north from its western end, and a rear building at the wing's northern end. The enclosed northeastern courtyard is visible through windows but inaccessible.

The foyer is a double-height hub with a grand staircase and views of the upper gallery and screening-room door. The dining room and library hold the warm-ups. Ground-floor progression continues north through wardrobe storage and fitting room to the workshop and prop store. A trolley provides access to the rear upper floor; the route then passes the gun cage, rehearsal hall, raised musicians' balcony, and west gallery, returning through the study to the upper foyer and screening room.

The west gallery is `36 × 6 m`, with eight west-facing windows and a `6 m` clear height. Its raised musicians' balcony is at `+8`, three meters above the gallery floor. The finale's shooting mark is `X (-17.5,35)`; player position is `S (-17.5,29)`. These are gameplay dimensions, not decorative suggestions.

Construction corrected the initial drawings and specification. **For coordinates, source §4.6 and `Tools/stage03_layout.py` take precedence over conflicting older values.** Significant corrections include:

- The dining room's eastern bay, `X[14.3,20]`, rises to `9.5 m`, providing clearance above its `+3.6` orchestra platform. The screening room above shrinks to `X[7.2,14.0]`.
- The workshop loading bay, `X[-12.9,-6.5] × Y[65.1,69.5]`, is open to the roof. Its `+3.6` mezzanine connects by a short stair to `+5.0`.
- Gallery sills were lowered to `0.3 m`; eight window centers use `Y = 17.7 + 4.0 i`. Light-pool centers on the corridor axis fall at `Y = 19,23,…,47`, placing mark `X` in the fifth pool and `S` in darkness.
- `M-A` moved from `Y = 41` to `Y = 39`: in a light pool, eight meters from `M-B`, four meters from `X`.

The drawing brief requested separate floor plans, a cutaway axonometric showing both floors and vertical connections, and a code-drawn scaled check plan. The source drawing table still labels these “pending”; it also references a later construction review. The references below identify the requested artifacts without claiming their validation status.

| Drawing | Referenced Artifact |
|---|---|
| Ground-floor plan | [L_Stage03_Plan_1F.png](images/L_Stage03_Plan_1F.png) |
| Second-floor plan | [L_Stage03_Plan_2F.png](images/L_Stage03_Plan_2F.png) |
| Cutaway axonometric: two floors, connections, twilight direction | [L_Stage03_Axonometric.png](images/L_Stage03_Axonometric.png) |
| Scaled check plan | [L_Stage03_Plan_Scaled.png](images/L_Stage03_Plan_Scaled.png) |

## Implemented State and Differences from Design

The source's 11/15-night status reports **scene and gameplay implemented**: puppets, pistols, rail trolleys, documents, doors, switches, plates, checkpoints, and exit—40 mechanisms across 12 rooms. Building shell, set dressing, lighting, and gameplay were connected. `Tools/build_stage03_gameplay.py` generates mechanisms with tag `S03Gameplay` and IDs in the 30xx range; it must run after shell and set dressing. It replaces mechanism-controlled shell doors (`SD` / `FR_N` / `GN` / `PD` / `DOOR48`) and the foyer gate with working doors.

| Item | Original Design | Implemented Difference |
|---|---|---|
| Pistol ammunition | No count | Six-round magazine; `F` reload takes `1.8 s`; unlimited reserve; empty gun only clicks |
| Gun model | Prop revolver | Engine shooting-template pistol and matching hold/draw/empty/reload animations; Tripo revolver unused because magazine-reload animation did not fit it |
| Firing animation | Not specified | Recoil return over holding idle, muzzle flash, tracer; template additive firing animation could not be layered because these arms did not use an Animation Blueprint |
| Trolley access | Walkable side steps | Two `87 cm` box steps behind the platform; three jumps to climb; platform length `1.6 m`, avoiding excessively steep steps and overhead collisions |
| Document interaction | Aim and `E` | Within `2 m` **and** looking toward paper within approximately `25°`, preventing documents from stealing interaction beside switches or guns |
| Take duration | Global five seconds | Every level-three room uses ten seconds and two reels; `P2` pushing alone takes `3.8 s` |
| Puppet movement | Straight lines | NavMesh navigation around furniture/screens; both west-wing floors have navigation |
| Fitting-booth entrance | `0.8 m` | `1.1 m`, allowing puppets inside so booths are not safe rooms |
| Red shot timeline ticks / pre-shot hammer sound | Planned | **Not implemented**, due to time |
| Insufficient-angle second-shot deflection / white flash | Planned | **Not implemented**; second hit has no effect and does not reset cracks |
| `P4` slow-motion camera | Optional | Not placed |
| Puppet resistance marker | Art to decide | Red chest light |
| Puppet / pistol audio | Dedicated effects | Existing-library substitutes, including typewriter keys for footsteps |

Placement changes moved `D1` to `(-4.9,1.2)`, `D2` to `(19.0,4.6)`, and `T1`'s starting position to mid-rail `x = 11.5`, avoiding furniture and levers.

A later source note changes the finale: **from 11/16, `DOOR-48` has no condition and remains open**, following the producer's request that players reach the exit without killing the puppets. `P4` is therefore optional. Older solution and anti-skip passages still describe requiring both puppets shattered; this selected account preserves the later explicit change.

## Verification and Limits

Module tests used PIE with `python Tools/s03_test.py <name>`, starting a separate PIE session for each test.

| Test | Observed Result |
|---|---|
| `doc` | Passed: gaze-dependent hint; `E` opens paper, pauses world, closes again |
| `stairgate` | Passed: upper-floor gate switch |
| `bust` | Passed: bust/person occupy plates; door stays closed without `L1` |
| `trolley` | Passed: push `T1`, climb via three jumps, reach `+3.6`, operate `L1` |
| `puppet` | Passed: `P1` dormancy, switch wake, gaze freeze, movement, booth entry and capture |
| `trolley2` | Passed: recorded watcher holds `M2` while player pushes `T2` to loading bay and climbs |
| `pincer` | Passed: single-hit healing, recorded first shot plus opposite second shot, shatter and balcony-door opening |
| `wake` | Passed: leaving gun cage wakes `M3`; exit triggers completion |
| `gun` | Passed: draw, six shots, empty state, `F` reload to six |
| `p4` | Partial smoke test only: north-door unbolt, `ROLL` wakes both, approach when unwatched, front puppet occludes rear |

**No continuous entrance-to-exit playthrough was completed.** The full `P4` solution, balcony gunfire, no-carry zone, original two-shatter door condition, front-stair one-way door, checkpoint restoration states, and possible softlocks were not tested. The source notes that door wiring shares `P3`'s logic; that is not an end-to-end validation.
