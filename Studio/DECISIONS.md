# Decision record

Only trade-offs that change direction (gameplay, scope, style, technical route), one per line.

These four are from the first day, when the game was still about cutting the world into film layers. That idea was dropped the same day for "record yourself and edit the footage"; later decisions were written into the hand-over log and the mechanics document instead of this table.

| Date | Decision | Why |
|---|---|---|
| Nov 12 | Enter with UE 5.9 (not UEFN) | |
| Nov 12 | Demo rooms are **assembled from several shot volumes (ShotVolume)** to limit how much one cut can cover (for example room 2 is split into 2A / 2B / 2C, with the stepping stones on the boundaries between volumes) | The vertical cutting plane passes through the camera and almost contains the view direction, so a cut strip is a corridor "infinitely long along the view direction and 10 m wide sideways"; the 10 m thickness alone cannot limit the distance going forward. Clipping by shot volume (already in GDD §3.4) makes "crossing a long distance takes several cuts in a row, with pressure from the context" hold without changing the rules |
| Nov 12 | The look of the three layers comes mainly from **procedural materials + layered post-processing** (`M_LayerSurface` / `M_PP_LayerGrade`), not from external scene packs | The geometry can stay grey-box; extreme colour grading is enough to tell three films apart. Clipping, cut edges, forgetting and streaming-in are all written in the same material function and match the code's region tests one to one |
| Nov 12 | Layer membership uses **Actor Tags** (`Layer.Base/West/Space`); no component is required | Levels are built in bulk by Python and tags are the least work; code and materials share the `MPC_CutRegions` contract |
