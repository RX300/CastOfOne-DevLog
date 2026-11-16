# CAST OF ONE — Development Log

Epic AI MegaJam 2026 · **Director’s Cut** · Team **RX300** · Unreal Engine 5.9

*You are the director. You are also the whole cast.*

CAST OF ONE is a first-person puzzle game built around recording actions, rewinding, and arranging takes on a timeline. Replay doubles perform those takes while the player acts alongside them.

This is a **curated English development archive**, selected from the working `Studio/` documents. It focuses on the development history, gameplay and rendering architecture, the DLSS port, and the three stages’ design drawings. The original department hierarchy and filenames are retained under `Studio/`.

The main handover log and engineering log are full translations of the selected source files. The three map documents are explicitly labeled **selected translated excerpts**. Historical plans, failed approaches and “not tested” statements remain historical statements; a design drawing is not evidence that every depicted feature shipped. Original tool/terminal `.log` files are retained unchanged in the project and are not included in this smaller reading package.

![CAST OF ONE thumbnail](images/CastOfOne_Thumbnail.jpg)

## Start here

| Document | What it contains |
|---|---|
| [Handover log](Studio/LOG.md) | The chronological development record, newest first, including failed attempts and verification limits |
| [Engineering log](Studio/Engineering/README.md) | Implementation rounds, tests, performance work and handoff notes |
| [DLSS port to UE 5.9](Studio/Engineering/DLSS_PORTING.md) | Plugin-level SR/DLAA, frame generation, multi-frame generation and Reflex integration; recorded build and runtime checks |
| [Mechanics](Studio/Design/MECHANICS.md) | Recording, rewind, editing, doubles, devices, checkpoints and later gameplay additions |
| [Decisions](Studio/DECISIONS.md) | Early direction-level decisions, preserved as historical records |
| [Release log](Studio/Release/README.md) | Packaging, size reduction, smoke tests and trailer production |

## Runtime architecture and collaboration

These two diagrams summarize the architecture and working process documented during the jam. They were regenerated from the existing project script `Tools/devlog/diagrams.py`.

![Runtime architecture](images/architecture_runtime.png)

![Human producer and AI-agent workflow](images/workflow_agents.png)

The human producer set direction, provided feedback and playtested. Agents worked through shared department documents, editor tools and scripts. As clarified by the producer for submission, the tools included Unreal Engine 5.9, Tripo, fal.ai, Claude Code (Opus 5.5 and Fable 5.2), Codex (6.1 Sol and Astra), and Antigravity (Argon and Hydrogen). ChatGPT and Codex were used during development for gameplay and rendering architecture, the UE 5.9 DLSS port, testing and performance optimization. Historical entries retain the agent names used when they were written.

## Stage 01 — Soundstage

[Selected design excerpts and implementation notes](Studio/Design/Maps/L_Stage01_Soundstage.md)

![Stage 01 topology](Studio/Design/Maps/images/L_Stage01_Topology.svg)

Early concept sheets, later superseded by construction and playtest revisions:

![Stage 01 concept overview](Studio/Design/archive/Stage01_concept_drawings/L_Stage01_Overview_Axonometric.png)
![Stage 01 concept topology and section](Studio/Design/archive/Stage01_concept_drawings/L_Stage01_Topology_And_Section.png)

## Stage 02 — Snow Valley

[Selected design excerpts and implementation notes](Studio/Design/Maps/L_Stage02_SnowValley.md)

The drawings record the design at their stated revision. Later scope cuts and the shortened ending are recorded in the development logs.

![Stage 02 scaled top-down plan](Studio/Design/Maps/images/L_Stage02_Plan_TopDown.png)
![Stage 02 puzzle flow](Studio/Design/Maps/images/L_Stage02_Flow.png)
![Stage 02 scaled sections](Studio/Design/Maps/images/L_Stage02_Sections.png)

Early concept sheets:

![Stage 02 concept overview](Studio/Design/archive/Stage02_concept_drawings/L_Stage02_Overview_Axonometric.png)
![Stage 02 concept topology and section](Studio/Design/archive/Stage02_concept_drawings/L_Stage02_Topology_And_Section.png)

## Stage 03 — Manor

[Selected design excerpts and implementation notes](Studio/Design/Maps/L_Stage03_Manor.md)

![Stage 03 vertical cutaway](Studio/Design/Maps/images/L_Stage03_Axonometric.png)
![Stage 03 scaled floor plans](Studio/Design/Maps/images/L_Stage03_Plan_Scaled.png)
![Stage 03 ground floor](Studio/Design/Maps/images/L_Stage03_Plan_1F.png)
![Stage 03 upper floor](Studio/Design/Maps/images/L_Stage03_Plan_2F.png)

## Early prototype — L_Backlot

These drawings show the earlier test map, not a stage in the final package.

![Backlot overview](Studio/Design/Maps/images/L_Backlot_Overview_Axonometric.png)
![Backlot key sets](Studio/Design/Maps/images/L_Backlot_KeySets_Cutaway.png)
![Backlot section profile](Studio/Design/Maps/images/L_Backlot_Section_Profile.png)

## Selected screenshots

| | |
|---|---|
| ![Main menu](images/shots/UI_Menu_Home.png) | ![Handbook popup](images/shots/UI_Manual_Popup_Keys.png) |
| ![Locked button](images/shots/S01_LockButton_Locked.png) | ![Unlocked button](images/shots/S01_LockButton_Open.png) |
| ![Exit beam](images/shots/S01_Exit_Beam.png) | ![Pause-menu handbook](images/shots/UI_Pause_Handbook_Keys.png) |

## Supporting records

| Document | Purpose |
|---|---|
| [Art direction](Studio/Art/ART_STYLE.md) | The visual direction and its acceptance criteria |
| [Interface design](Studio/Art/UI_DESIGN.md) | Camera-body HUD, film terminology and menu design |
| [Audio log](Studio/Audio/README.md) | Sound/music generation and integration record |
| [Tools and asset credits](Studio/Release/CREDITS.md) | Historical asset/source/license inventory; retains its original pending checks and queued-asset notes |
| [Pre-jam declaration](Studio/Release/PRE_JAM_DECLARATION.md) | What was prepared before the jam |

The source of each document is the same path in the project’s original `Studio/` directory. References to scripts, assets and historical documents outside this selection are project references, not bundled files. Public repository: [RX300/CastOfOne-DevLog](https://github.com/RX300/CastOfOne-DevLog). This archive does not itself establish that the game was submitted to the jam.
