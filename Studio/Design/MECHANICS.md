# Mechanics Rules

> **Version v1** (2026-11-14). This is the only document for the current rules; what is implemented in the game is what is written here.
> The version number is only bumped when the rules change a lot, or when the producer says so; ordinary edits go straight into the body, with a line added to the change log at the end.
> The hard rules for puzzle design are in SOFTLOCK_RULES (`SOFTLOCK_RULES.md`), the puzzle library is in PUZZLES (`PUZZLES.md`), and the demo map is in Maps/DEMO_L_Backlot (`Maps/DEMO_L_Backlot.md`).

## 1. In one sentence

You are the director, and also your own actor. Record a few seconds of yourself, and you can rewind to undo those seconds while the film stays; at the editing desk, lay the film on the timeline, call ACTION, and a double does it all again for you: dies for you, carries props, gives you a leg up, acts as bait.

The one sentence for the player: **While the camera is rolling you are a double, and if you die you can rewind; while it is not rolling, dying is dying for real.**

## 2. Controls

| Key | Action |
|---|---|
| `WASD` + mouse | Walk, look |
| `Shift` | Run (5 seconds of stamina, see §17) |
| Space | Jump (you cannot jump while holding a prop, except the pistol; you cannot jump while pushing the trolley) |
| `Ctrl` hold / `C` toggle | Crouch |
| `E` | Pick up / put down a prop, throw a breaker, open a locked button while holding a key, place a prop on a stand; facing a document it reads it; at the trolley handle, **holding** it pushes; when the handbook pops up it closes it |
| Left mouse button | Fire (when holding the pistol) |
| `F` | Reload (when holding the pistol) |
| `R` | Roll camera / stop the take |
| `Q` | Rewind (within 3 seconds after stopping the take) |
| `TAB` | Open / close the editing desk |
| `G` | ACTION / CUT |
| `Esc` / `P` | Pause menu (resume / back to last checkpoint / handbook / settings / main menu / quit) |

The editing desk uses only the mouse: left button drags takes and clips, the right-click menu splits / deletes, and holding the right button and dragging on the picture lets you turn your head and look around.

## 3. Recording

- `R` rolls the camera. At that moment a snapshot of the **whole map** is taken (called "position one" below): your position, the positions of all props, the state of doors and mechanisms, the guards, the edit, the world clock.
- What is recorded: per-frame position, facing, pitch, crouch, run, whether there is something in your hands, and the actions done with `E` (throwing a breaker, placing on a stand).
- A single take is at most **5 seconds** long. There are three ways the take stops: pressing `R` again, the take filling up, and death. All three are saved as a Take.
- Recording can cross rooms: walking into another room after rolling the camera does not stop the take.
- While recording you can be burned by fire, hit by the train and caught by guards just as usual.

### Film

- **Reel slots are shared across the whole map**; all rooms use the same footage library and the same capacity; changing rooms neither adds to the number of takes you can record nor clears the footage.
- **Every stage has 2 slots from start to finish** (there is no save file and upgrades cannot carry over to the next stage, so the 1→2 slot upgrade in Stage 01 and the equipment room unfreeze upgrade in Stage 02 were removed). The capacity is still saved and restored with checkpoints.
- **You cannot start recording with the slots full**; a prompt tells you to delete one at the editing desk first; a single slot is no longer overwritten automatically either. Cancelling a recording does not delete existing footage.
- A Take is not used up; it can be played and split again and again. The set number in the name only says where it was shot; the slot number is unique within the whole-map footage library; the editing desk count and the recording check use the same whole-map total.

So "I need two helpers at once" has only three ways out: one recording doing two jobs, splitting one recording and stacking it on several tracks, or letting a prop or a set device hold one position for you.

## 4. Rewind

- However the take stopped, there is a **3-second window** afterwards. Within the window press `Q`, or click rewind at the editing desk. Time is paused in the editing desk, and the countdown stops too.
- **Rewind**: the whole map returns to position one, including the world clock, the edit, and whether it is playing. The Take just shot stays in the footage bin.
- **No rewind**: the window closes, and what just happened is real.
- Only the Take just shot can ever be rewound. Once the window closes there is no rewinding it again.
- Only what the double does is real: go and die yourself and then rewind, and the guards and mechanisms all go back with you. The exception is anything lit by a key light, see §9.

## 5. Editing desk

- While it is open with `TAB` **the world is paused**; press again to close it and the world carries on.
- **The whole map shares one editing desk**: the three tracks, the footage bin and the undo history are not split by room. Leaving a room clears nothing; things only go away when the player deletes them (deleting a clip, right-clicking a track name to clear it, deleting a Take).
- **Placing a clip**: drag a Take from the footage bin onto a track. A clip's position is "how many seconds after ACTION is called", not world time. The timeline shows from 1 second before ACTION to 15 seconds after.
- **Changing a clip**: drag with the left button (horizontally to change the time, vertically to change the track); the right-click menu splits / deletes. Clips on the same track cannot overlap.
- **Undo**: undoes the last edit step. It only undoes the edit, not things that have already happened.
- **No preview**: dragging the playhead only shows where the doubles will be at that moment, not how the world will change.
- The timeline is read-only during playback; call CUT first to change it.

## 6. ACTION

- **Standby**: the clips that have been laid out do not play.
- **`G` = ACTION**: starting from second 0, all clips play together, and the doubles act at the positions each was recorded at, no matter which room you are in.
- **Pressing `G` again during playback = CUT**: all doubles leave the set at once, and whatever is in their hands drops at their feet.
- After playback ends or CUT is called it returns to standby, **the edit is kept as it is**, and pressing `G` again replays it.
- Recording and playback do not get in each other's way: you can record a new Take while an old one is playing.

## 7. Doubles

A double follows the same rules as you: it can step on plates, carry props, be burned by fire, hit by the train and caught by guards. There are three differences: it only moves as the recording says; when it dies only that clip is ruined; and it is killed by a closed door (a recording cannot pass through a door, so the clip ends when it meets a closed door). **When you yourself run into a closed door you are just blocked; you do not die.**

- **Pick up**: a double only reaches out at two moments. One is the moment it appears (in this frame of the recording you have something in your hands), the other is the moment you picked something up in the recording. When it reaches out it takes whichever prop is in reach; if none is in reach it finishes the walk empty-handed. The recording only notes "reach out here", not which one was taken.
- **Put down**: it puts down at the moment you put down in the recording. When a double dies, finishes playing or is cut, what is in its hands drops at its feet.
- You can take things from a double's hands; a double does not take things from yours.
- **Doubles are solid**: they block the way, block guards, block the key light's light, and can be stood on. A crouching double's head is about 1.1 m up, and stepping on it and jumping gets you onto a ledge of a bit over 2 m; a standing double is about 1.9 m, and you cannot jump onto it.
- If a double overlaps you when it appears, it only becomes solid once you step away.
- Throwing a breaker and placing a prop on a stand in the recording are done again by the double on replay.
- The slow-motion camera does not affect doubles (§9).

## 8. Props

- You can only hold one at a time. `E` picks up the prop under the crosshair that is within 2 m of your eyes and not blocked. You cannot pick up while airborne.
- Keys and crates are the same kind of thing: crates hold down pressure plates, keys open locked buttons (§10).
- **Everything usable has an outline**: props, breakers not yet thrown, locks not yet opened, documents, the trolley and the camera not yet picked up always show a breathing amber outline (fading out beyond about 26 m; parts hidden behind walls are not drawn); the one under the crosshair changes to a brighter warm white.
- Props are conserved: none are added, and none disappear.
- **Holding**: the prop floats 110 cm ahead of your line of sight and 55 cm below it, raised when you look up and lowered when you look down; it stops when it meets a wall, glass or a closed door. A double uses the recorded line of sight and holds it at the same height.
- **Putting down = placing on the ground in front of your feet**. The landing spot is the ground 85 cm in front of you; if that is not the level of ground you are standing on (a higher step, a pit), or something is in the way, it lands at your own feet. So you cannot climb steps while holding something.
- **The stand** is the only place where a prop can be put somewhere else: with something in your hands, the stand empty and its top within 2 m, press `E` to place it on top. One stand holds one item.
- **No-prop zone**: a stretch of ground props cannot enter. Walk in holding something and it drops at the boundary; you cannot pick things up inside either. It applies to doubles as well.
- **Props do not leave the room**: when you step into another room holding something, it stays at the doorway of the original room.
- When killed by fire, the boulder, the train or a guard, what is in your hands is thrown 0.8 m forward; in other cases it drops at your feet.

## 9. Set devices

- **Key light**: a fixed lamp on the set, always on, with a warm cone of light. Props and guards lit by it at the moment `Q` is pressed **do not rewind**; they keep their position and state at that moment (whether a guard is alive or dead is kept too). If a lit prop is being held, after the rewind it lands at the holder's feet.
  - "Lit" = inside the cone (60°, 16:9), within range, with no wall, closed door or double between the lamp and the object. Glass does not block. Lit things have an outline.
  - Loading a checkpoint is not protected by the light.
- **Slow-motion camera**: a camera on a tripod, always on, with a blue lens range. **You and the guards** inside its lens act at 45% speed; **doubles are not affected**. What you record inside the lens is itself slow, and replays at that speed too.
  - The test for "inside the lens" is the same as for the key light: inside the lens range, with no wall, closed door or double between the lens and you (glass does not block). So a double standing in front of the lens can block the people behind it back to normal speed. Your own body does not block.

Puzzle design: the light only shines on "the landing spot of the thing that is meant to stay"; do not put a key and its lock one inside the light and one outside. The slow-motion lens is used to pull apart the speed of living people and of the film.

## 10. Mechanisms

| Mechanism | Rule |
|---|---|
| Pressure plate | Counts as held down when a person (you, a double) or a crate is on it. Guards do not count |
| Door | Opens only when everything connected to it is satisfied, otherwise closed. It can be connected to pressure plates, breakers, locks and guards (opens only when the guard is dead), and also to breakers and locks in other rooms |
| Breaker | Press `E` within 2 m to throw it; once thrown it does not open again by itself. Throwing it is recorded into the Take |
| Lock (locked button) | A red button on a post, under a glass cover with a padlock. Press `E` within 2 m **while holding a key**: the padlock disappears, the glass cover flips open, it counts as pressed, the key is used up, and it never locks again. Pressing `E` empty-handed only shows "LOCKED - you need a key". This key press is recorded into the Take, and a double does the same when it has a key in its hands; a double holding a key that touches the lock opens it too. The key itself touching the post (thrown at it, placed at its foot) also counts as unlocking; this is the old rule, kept |
| Fire jet | Fires on a cycle, or only when someone comes near (sensor). It can be connected to pressure plates or breakers, and stops firing when everything connected is satisfied |
| Boulder | After being triggered by a trip line or a sensor zone it rolls down a fixed route, and returns to its start after a few seconds |
| Train | Runs along the track on a cycle; if the switch plate is held down when the engine passes the switch it turns onto the branch line and smashes the wooden door |
| Trip line / sensor zone | Triggers the mechanism connected to it when you or a double pass. Guards do not trigger it |
| Pit, high threshold | A one-way drop. An empty-handed person can climb out by stepping on a crouching double; a person holding something cannot get out |
| Exit | A standing beam of light: dark amber when not open, bright green when open. Once it is open, walking in clears the stage: all controls are locked, "THAT'S A WRAP" appears, and after 4.5 seconds it returns to the main menu |

Fire, the boulder and the train kill on touch, the same for you, doubles and guards.

## 11. Guards

There is no attack key; you deal with them entirely through the existing mechanics.

- They stand at a post or patrol a fixed route, and their field-of-view fan is visible on the ground.
- They chase when they see you **or a double that is playing** (they cannot tell the difference). They chase faster than walking and slower than running. When they get right up to you they catch you:
  - Catching you: while the camera is rolling it is an NG and you can rewind; if it is not rolling you go back to the last checkpoint.
  - Catching a double: that clip is ruined, and what is in its hands drops at its feet.
- After losing track they walk to the last place they saw you, look around for 2 seconds, then walk back to their post the way they came.
- They do not go down pits, do not climb ledges, cannot pass closed doors, are blocked by solid doubles, and cannot leave the room they belong to.
- They do not hold down pressure plates and do not trigger trip lines, but they do trigger sensor fire jets.
- They can be killed by fire, the boulder and the train. Dead, they stay where they are and no longer block the way. A door can be connected directly to a guard: when he dies the door opens.

## 12. Death

- **Death while recording**: the recording stops at the frame of death and is saved, and the take is marked NG. The picture freezes with a 3-second countdown, and this is the rewind window.
- **Death within the 3-second window after stopping the take**: it enters the NG freeze in the same way, and you can still rewind for as long as is left of the window.
- **Death when the camera is not rolling**: "CUT!" and the cause of death appear on screen, and after about 1.8 seconds you go back to the last checkpoint (§13).
- When the window runs out after an NG: back to the last checkpoint.
- Only fire, the train, the boulder and guards can kill.

## 13. Checkpoints

- **Checkpoints are placed by hand in the level** (`BP_Checkpoint`, a box volume). There is no automatic save per room. At the start of the game it saves once automatically at the spawn point.
- **Each checkpoint saves only once**: at the moment the player first walks into it. Walking back into it afterwards does not save again; only walking into the next checkpoint not yet reached makes a new save, which overwrites the old one. The whole game has only one save.
- If you are recording when you walk into a checkpoint, it saves after the take stops, while you are still inside the volume.
- **What is saved is the state of the whole map**: all mechanisms and props, breakers and locks, guards, every Take and the edit, your position.
- **There are only two ways back to a checkpoint**: death (§12), or the player choosing it in the pause menu (`Esc` / `P`) or with the `LAST CHECKPOINT` button in the editing desk. There is no hotkey. After going back everything is restored to how it was at the moment of the save; Takes recorded, clips laid out and breakers thrown after the save are all gone.
- This is a safety net, not part of the solution. How checkpoints are placed is in SOFTLOCK_RULES §3.7 (`SOFTLOCK_RULES.md`).

## 14. The whole map is one whole

- The whole map is always running. In a room you have left, the mechanisms carry on as usual, and doubles that are playing finish their performance as usual.
- Leaving a room triggers no reset of any kind: no CUT, doubles do not leave the set, the edit is not cleared, guards are not put back at their posts.
- A room decides only these things: which room's film is used when you roll camera here, the director's line on entering, the guards' range of movement, and that props cannot be taken out.

## 15. Facts you can rely on when designing puzzles

1. A double can only go where you have been, and can only carry what you could carry there.
2. A rewind undoes at most 5 seconds. Progress from before those 5 seconds is fixed.
3. After a rewind the world is exactly as it was at the moment the camera rolled (except what the key light is shining on). Lay that Take out and call ACTION, and the double will replay what just happened exactly.
4. Props are conserved. There is no such thing as "the door is open and the key is back in my hand".
5. In one Take a double walks at most about 21 m, and runs about 34 m. Room scale follows this.
6. What a double carries stays moved for good, so **the key light is only irreplaceable when "a double cannot get in"**. The most solid construction for a single-slot room: getting in needs a double holding down a pressure plate, so the one who goes in to do the work can only be you in person, and when done you can only rewind out; at that point the only way for something to stay is the light.
7. A pit is not an absolute one-way street: an empty-handed person can record a crouching leg-up clip and climb out. Only a person holding something truly cannot get out.
8. In a 2.4 m wide corridor nobody can squeeze past a guard; a dead end is a dead end for the living, but not for a double.
9. Pick-up distance is measured from the eyes: at the bottom of a 1.5 m deep pit, crouching at the pit's edge you cannot reach something on the floor, but you can reach something a double is holding at its chest.
10. The editing desk does not clear itself. When the player enters a new room, clips from other rooms may still be on the tracks, and they will play too when `G` is pressed.

## 16. Pitfalls to avoid when designing puzzles

**A "walk in and die" trap does not make a puzzle.** Wherever the double dies, the thing drops there; but a living person can also walk to the edge of the same spot, put the thing down and walk away. A trap must be something **a living person cannot deliver to, and only someone who does not plan to come back can**:

- **Timed corridor**: the fire jet stays off for too short a time for a living person to make the round trip; a double does not need to come back, so it delivers deeper.
- **One-way door**: once in you cannot get out.
- **Two-stage**: entering the corridor triggers a boulder that chases from behind, forcing you to run forward, with a fire jet at the end. Recording only the last half second does not help; after the rewind the boulder is still behind you.
- **One-shot trap**: whoever steps on it dies, and once stepped on it is spent. This is a different kind of puzzle (a double clearing the mines).

**An empty-hands passage must be longer than reach.** A low wall will not do: put the thing at the foot of the wall, jump over, then reach back for it from the other side. Use a no-prop zone of 3 m or more.

**"You can jump across" does not mean "the thing cannot be carried across".** To block props, use a no-prop zone or a gap of 4 m or more.

**No softlocks.** See SOFTLOCK_RULES (`SOFTLOCK_RULES.md`).

## 17. Numbers

| Item | Value |
|---|---|
| Walk / run | 430 / 680 cm per second |
| Jump | Peak 113 cm, about 0.7 seconds in the air; walking jump about 2.9 m, running jump about 4.6 m |
| Eye height | About 174 cm standing, about 92 cm crouching |
| Single Take | 5 seconds (10 seconds in Stage 03, set per room) |
| Rewind window | 3 seconds |
| Pick-up distance | 200 cm from the eyes |
| Hold point | 110 cm ahead, 55 cm down |
| Put-down landing spot | 85 cm in front |
| Stamina | A full bar gives 5 seconds of continuous running; recovery starts 0.6 seconds after you stop running and refills in 3 seconds; after running it empty it must recover to 30% before you can run again. Shown as a 10-segment ring outside the crosshair, 0.5 seconds per segment |
| CUT screen on death when not rolling | 1.8 seconds |
| Guard | Chase 520, patrol 260 cm per second; field of view 100°, 12 m; looks around for 2 seconds |
| Key light cone | 60°, 16:9 |
| Slow motion | 45% |
| Puppet | Chase 450, return to position 260 cm per second (you walk 430, run 680); view test: a 16:9 frame 100° wide, any one of 15 points on its body visible; within 90 cm counts as caught; crack window 0.6 seconds, angle between the two shots ≥ 120° |
| Pistol | 6-round magazine, 1.5 second reload, 0.58 seconds between shots, can pull the trigger again 0.35 seconds after the empty click; range 80 m |
| Trolley | Pushed at 250 cm per second; platform top 2.6 m, 1.6 m long, the two steps behind are each 87 cm high and 60 cm deep; the handle test is 5–150 cm beyond the rear of the cart |

All these values are adjustable in Blueprints and data assets; where they are is in the Engineering README (`../Engineering/README.md`).

## 18. Stage 03 devices: puppet, pistol, rail trolley, document

Implemented on the night of 11/15; the code is in `RoomSimulation` (rules) and `ManorMechanisms` (the presentation of the puppet and the trolley). The design motivation is in the Stage 03 map document §3 (`Maps/L_Stage03_Manor.md`).

### Puppet

- **It does not move while being watched.** Watched = **any part** of its body appears in your picture: 5 × 3 points are taken on its body (head to feet, left, centre, right), and as long as one point falls inside the frame (a 16:9 frame 100° wide, a little wider all round than the actual picture) and nothing blocks the line from the eyes to that point, it counts as watched. One glimpse freezes it for 0.2 seconds, so when it is behind railings or a door frame it does not shuffle forward in stops and starts. Walls, closed doors, the trolley, other puppets and doubles all block; glass does not; there is no distance limit. A double that is playing has eyes too.
- **It walks when nobody is watching**: towards the nearest living person **in its own room** (you, or a double that is playing), going around furniture along the navmesh; it does not climb ledges, does not go down pits, and cannot leave its own room. Inside the slow-motion lens it drops to 45% all the same.
- **Within 90 cm it catches you** (only if the height difference is within 1.5 m, so the trolley's second step and its platform are safe, and the first step is not). Catching you is death; catching a double ruins that clip and what is in its hands drops at its feet. A frozen puppet is a solid thing; it blocks the way and does no harm.
- **It only falls for it once**: within one ACTION, a double's eyes can freeze it, and a double can also lure it away; once that ACTION ends, a puppet that was frozen by a double or chased a double has **seen through it** (a red light comes on at its chest), and after that it only recognises you in person. Shots fired by a double always work.
- **It forgets when it returns to position**: when there is no target in its room it walks back to its original position, and forgets having seen through it at the same time. So leaving the room and coming back lets you start over.
- **Dormant**: it can be connected to breakers or sensor zones; when any one of those connected is triggered it wakes, and once awake it does not sleep again. One with nothing connected is awake from the start.
- **Only a crossfire can shatter it**: the first shot cracks it open for 0.6 seconds (it glows; this is the window), and only another hit from the opposite side during that time (angle between the directions the two shots came from ≥ 120°) shatters it; otherwise the crack heals. A door can be connected to "the specified puppet is shattered".
- Rewinding and loading a checkpoint restore everything about it: position, dormancy, having seen through it, shattered or not.

### Pistol

- A prop, a light one: you can run and jump while holding it. One item at a time, and it cannot enter a no-prop zone. When you hold it it is shown in your hands (first-person gun hold); when a double holds it it is gripped in the right hand, using the armed stand / walk / run animations, the muzzle kicks up when it fires, and the muzzle flash and tracer line come out of its muzzle.
- Left button fires, hitscan, stopping at the first thing in the way; **you and doubles can be shot dead too** (cause of death "Shot on your own set").
- **6-round magazine**; when empty there is only the empty click; `F` reloads in 1.5 seconds (the reload animation at its original speed), during which you cannot fire; you can reload whenever it is not full, and reserve ammo is unlimited. The round count goes with the gun, and goes back with it on a rewind.
- **One gun has 0.58 seconds between two shots** (that is the length of the fire animation), and it is not reset by changing who holds it or by putting it down and picking it up again. The puppet's crack only stays open for 0.6 seconds, and with one gun there is no time to get round to the opposite side for the second shot, so a crossfire still takes two guns: one for you, one for a double.
- Firing and reloading are both recorded into the Take, and the double fires on the same frame from its eyes along the recorded line of sight; **if the double has no gun in its hands nothing happens** — after a rewind the gun is back in your hands, and you have to press `E` to put it at the double's starting point first, so the double picks it up when ACTION is called.
- UI: a thin-line crosshair (spreading when firing and moving), a diagonal hit marker for hitting a puppet (red when it shatters), a round count panel at the bottom right, reload progress and the empty-magazine prompt below the crosshair, and `LMB` / `F` in the keycap bar at the bottom.

### Rail trolley

- It runs along the rail on the floor between two end points and cannot leave the rail. Stand at the rear of the cart (behind the crate steps) or outside the front of the cart, facing the body, and **hold `E`** to push; let go and it stops; it can only be pushed, not pulled. While pushing you move at sixty percent of walking speed, and cannot run or jump.
- The platform top is 2.6 m high, and jumping from the platform gets you onto a 3.6 m ledge; the two crate steps at the rear are each 87 cm high, **one jump per step** up to the platform. Two crouching doubles stacked cannot reach 3.6 m, so only the trolley gets you onto this kind of ledge.
- Solid: it blocks the way, blocks puppets and blocks line of sight (a double's line of sight too). It does not take up your hands and does not count as a prop. A double pushes as the recording says. It returns to position one with a rewind.

### Document

- Within 2 m, with your line of sight on the paper, press `E` to open it: a full-screen page, **the world is paused**, and pressing `E` again puts it back. Reading does not collect it, and there is no checklist.

### Door conditions

- Besides "everything connected is satisfied", a door can also be connected to a second group that "opens when any one is satisfied" (the Stage 03 service door: two pressure plates + the orchestra pit breaker, or the breaker on the inside of the door). One-way door = a door + a latch breaker that can only be reached from one side.

## 19. Director's handbook (tutorial pop-ups)

- 19 pages in all: basic controls, recording and rewind, editing and ACTION, a leg up (A LEG UP), pressure plates, breakers, keys and locked buttons, crates, fire, dying on purpose (DYING ON CUE), guards, key light, boulder, no-prop zone, slow-motion camera, stand, pistol, puppet, rail trolley. The text is in `Editing/UI/Manual.cpp`.
- **It pops up on first encounter**: basic controls pops up at the start of the game; recording and rewind pops up when you get the camera; editing and ACTION pops up when the first Take is saved (after the rewind window ends); for the rest, each kind of mechanism pops up its page the first time it is within 7.5 m (14 m for guards and puppets) and visible. **A leg up** and **dying on purpose** are techniques, not mechanisms, and pop up by location: a leg up pops up at the entrance to the audition pit in Stage 01 and at the 08 boost pit in Stage 02 (record a clip crouching, then after ACTION step on the double to get onto the ledge); dying on purpose pops up at the entrance to the fire wall in Stage 01 (walk into the trap holding a prop and record it, rewind, go to the other side and call ACTION; the double dies for you, and the prop it throws out stays). The world is paused while it is up, and `E` / Space / left button closes it (key presses within 0.6 seconds after it pops up do not count, so that a key already held down does not close it). It does not pop up while recording, during playback, within the rewind window or while airborne; it waits for these to end and then pops up.
- Each page pops up only once per game session, shared across the three stages; restarting the game makes them pop up again.
- `HANDBOOK` in the main menu and `HANDBOOK` in the pause menu let you page through all 19 pages at any time.
- Settings → GAMEPLAY → `HANDBOOK POP-UPS` can turn the pop-ups off (on by default); with them off, the handbook in the menus can still be read as usual.
- The key page states: you cannot jump or climb while holding a key; to get past a place you cannot jump across, record a Take that carries the key over, go around to the other side empty-handed yourself and then call ACTION, and take it from the double's hands; the double only remembers "hands are full", and at that moment of playback takes whichever prop is in reach. The pistol page states: the Take records the moment of every shot, but the double only really fires if it has a gun in its hands — after a rewind the gun is back in your hands, and has to be put at the double's starting point first.

## Change log

| Version | Date | Content |
|---|---|---|
| v1 | 11/14 | Merged the rules previously scattered across several documents into this one: the living world and the editing desk (formerly Demo v5), rewind and double carrying (formerly Mechanics v6), the ACTION key, key light, slow-motion camera, guards, prop holding and placing and the stand (formerly Mechanics v7), plus the whole-map editing desk and checkpoints decided on 11/14. The old documents are in archive/ (`archive/`) |

- 2026-11-15: The producer confirmed the whole-map reel slot rule; removed the out-of-sync per-room count and single-slot overwrite clauses.
- 2026-11-15: §9 now spells out the test for the slow-motion lens being blocked by walls / doors / doubles (the code `AStageSession::InCone` has long worked this way, and Stage 02 Q13/Q17 depend on it).
- 2026-11-15 evening: The producer reported "getting close to a door automatically sends you back to the checkpoint" — the cause was that the player touching a closed door counted as death; changed so that only doubles are killed by doors (§7). Removed long-press `T`; going back to a checkpoint is only by death or by choosing it in the pause menu (§13); added the pause menu and stamina (§2, §17).
- 2026-11-15 night: Added §18 (Stage 03's puppet, pistol, rail trolley, document, the any-one condition for doors), added left button / `F` / hold `E` to §2, and the matching values to §17; the pistol was changed to a 6-round magazine + reload at the producer's request.
- 2026-11-16: Reel slots unified to 2 slots throughout, upgrade points removed; guide lines added to the floors of the three stages (for § guide lines see the Engineering README).
- 2026-11-16 early morning: The producer reported "I can see most of the puppet in the picture and it is still moving" — it used to test only one point at the chest; changed to any one of 15 points on the body being inside the frame and not blocked (§18). Puppet chase 550 → 450. A double holding the gun now grips it in its hand.
- 2026-11-16 early morning: The pistol was swapped for the Retro Weapon Pack; the producer asked for the firing interval to follow the animation rather than 1 second: interval between shots 1.0 → 0.58 seconds, reload 1.8 → 1.5 seconds (§17, §18).
- 2026-11-16 04:30: When you enter another room holding a prop, the prop is put down behind you (staying in the original room) and a prompt pops up; it used to be put down in front, where it would land in the new room and could not be picked up again; picking up a prop no longer needs you to face it squarely, the nearest one within 15° of the crosshair will do. Stage 03 `DOOR 48` no longer requires shattering the two puppets in the gallery.
- 2026-11-16 06:00: The producer asked to remove "a prop is put down when you enter another room holding it". Props can now be carried to any room, and the same goes for double playback; wherever the text says "props cannot leave their own room / drop on entering another room", this takes precedence. No-prop zones (NoProps) are unchanged.
- 2026-11-16 06:30: Interaction rework requested by the producer — the lock was changed to "an upright button with a glass cover and a padlock", opened by pressing `E` while holding a key, with pressing `E` empty-handed prompting for a key (§10); usable things have an always-on outline (§8); the exit gained a beam of light, and after clearing the stage the controls are locked and it returns to the main menu (§10); added the §19 director's handbook pop-ups.
- 2026-11-16 07:00: The handbook gained the two pages "a leg up" and "dying on purpose", which pop up by location; the key page gained "cannot jump" and "use a Take to pass the key across"; the handbook pop-ups can be turned off in settings (§19). The lost mapping for the pause keys `Esc` / `P` was restored (§2 unchanged).
