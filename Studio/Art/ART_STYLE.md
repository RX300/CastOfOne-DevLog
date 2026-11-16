# Visual art direction · A realistic film set

> Status: **On 11/15, the producer explicitly requested a move toward high-quality realistic visuals.** Stage 01 uses believable industrial architecture, natural PBR materials, real equipment and indoor lighting with identifiable sources; Stage 02 uses daytime sunlight/skylight and exposure adaptation between interiors and exteriors. For the interface, see [UI_DESIGN](UI_DESIGN.md).
> Enabling Lumen or replacing textures with 4K versions does not make the result AAA. There are still too many basic primitives and repeated components, and a gap between the characters and the overall density of the art. Continue assessing the actual game image.
> This document sets only the overall visual style. Level construction, prop placement and writing on walls belong to level design.
> The producer and back-end team decide how to implement rendering technology separately. This document describes how the image should look.

## 1. In one sentence

**A believable soundstage: equipment has a purpose, buildings are traversable, materials are recognizable, and cinematic mechanisms operate in real spaces.**

The direction is now a realistic industrial film set. Preserve gameplay readability, but do not hide missing architecture and assets behind huge empty walls, uniform gray or excessively bright skylight.

| Dimension | Direction |
|---|---|
| Lighting | Realistic |
| Materials | Distinguish substances; preserve scanned base color, normals and roughness |
| Forms | Include supports, thickness, connections and maintenance access |
| Color | Natural wood, paint, metal and concrete colors, coordinated by area |
| Characters | Silhouetted; no realistic faces |

Prioritize clearly licensed scans and established industrial models. Dress work areas, storage areas and maintenance areas; do not randomly fill passageways.

## 2. Three principles

| Principle | Meaning | Check |
|---|---|---|
| **Recognizable mechanics** | Consistent gameplay signal colors; natural environmental colors. Mechanisms also use shape and motion. | Interactables remain distinguishable from dressing without relying on color alone |
| **Light takes the lead** | Separate visual layers through light and dark rather than color and detail. Key lights, camera ranges and guard vision are themselves light-based mechanics, so the environment must darken to make room for them. | Desaturating a screenshot still leaves subjects, paths and hazards distinguishable |
| **Purpose and hierarchy** | Concentrate detail in work areas and joints, keep paths clear, and provide moderate wall/floor variation. | No unexplained empty rooms, floating valves or equipment intersecting stairs |

## 3. References

Take principles only; do not copy specific images.

| Source | What to take |
|---|---|
| Gregory Crewdson's staged photography | Ordinary places at dusk lit cinematically: cool skylight plus localized artificial light |
| Jacques Tati's *Playtime* | Color as a rare accent in a gray-blue world |
| *Control* | Large surfaces, restrained UI, one consistent accent color |
| *Mirror's Edge* | A neutral world in which color is navigation |
| *Portal 2* | Readable puzzle spaces |
| *INSIDE* | Compose through value rather than color |

## 4. Palette

### Color structure

The following is a reference for coordinating colors; the environment no longer has to be nearly achromatic:

| Share | Role | Colors | Saturation |
|---|---|---|---|
| 70% | Primary | Cool gray, warm off-white, slate blue | Very low |
| 25% | Secondary | Dark metal, wood | Low |
| 5% | Accent | Mechanic colors | High |

If accents occupy more than one tenth of a screenshot, there are too many.

### Background: sky

Blue hour, after sunset.

| Position | Color |
|---|---|
| Zenith | `#1E2632` |
| Mid-sky | `#2B3442` |
| Low sky | `#4A5A6B` |
| Horizon | `#6F8496` |
| Last bright line against the horizon | `#A9B4BC` |

- No sun and no sunset glow. **No warm colors in the sky**; reserve them for key lights.
- Few clouds, thin and horizontal.
- Distant scenery is a silhouette one value darker than the horizon, with no internal detail.

### Environment

Ranges rather than exact colors for every surface:

| Category | Range | Example |
|---|---|---|
| Light surfaces (walls, panels) | Warm off-white to cool mid-gray | `#C9C6BE` – `#9DA3A8` |
| Ground | Dark gray, darker than walls | `#3A3D42` – `#55585C` |
| Dark structures (metal, frames) | Near-black charcoal | Around `#1C1E21` |
| Wood | Desaturated wood tones | Around `#8E7A5E` |

Avoid large areas of pure black or white. Allow natural colors from different materials; do not paint the same dado stripe everywhere.

### Mechanic colors

Each color means one thing. Use the same values in the UI and 3D world.

| Mechanic | Color | Character |
|---|---|---|
| Key light (protection) | `#FFC86B` | Warm gold, stable, with a clear beam; ordinary warm-white lights must not impersonate protected areas |
| Slow-motion camera | `#9FE6FF` | Ice blue |
| Danger: guards, fire, NG | `#FF4D4D` | Red; fire leans orange `#FF8A3D` and flickers |
| Recording, playhead | `#FF5A3C` | Orange-red, mainly in the UI |
| Doubles V1 / V2 / V3 | `#2EC4B6` / `#9BD35A` / `#F06BA8` | Teal / yellow-green / pink, matching timeline tracks |
| Completion, open gate | `#5BE37D` | Green |

- Warm colors have two meanings, distinguished by form: stable gold means protection; flickering reddish light means danger.
- Distinguish ice blue from the sky through value: the sky is dark and gray; slow motion is bright and pure.
- Every mechanic color also has a shape, icon or text cue; never rely on color alone.

## 5. Light and shadow

- **Time of day**: retain blue hour outside Stage 01; interiors use neutral-white and warm-white work lighting from real fixtures. Stage 02 uses daytime as requested by the producer.
- **Three kinds of light**:

| Light | Appearance | Purpose |
|---|---|---|
| Skylight | Cool, weak | A blue-gray base in shadows so they are not pure black |
| Environmental fixtures | Neutral or warm white, currently about 3800–4900K in Stage 01 | Illuminate routes, equipment and work areas without impersonating protective beams |
| Mechanic lights | Mechanic colors | Gameplay itself |

- **Environmental lights are not colored, flickering or moving**; reserve all three for mechanics.
- **Bright areas are where players should look**: light puzzle areas and darken transitions. From a distance, the lit areas should resemble a chain of small stages.
- **An unreadable route is a bug, not atmosphere**: floor edges, steps and pit openings must be visible wherever players can walk.
- **Exits are brighter than their surroundings**: eyes are drawn to the brightest area.
- **Air**: a very thin fog layer makes distant objects paler and bluer, separating depth. Only key lights have visible beams.
- **Shadows**: broad lights cast soft shadows; key lights cast hard shadows. This difference in character also identifies mechanics.

## 6. Material qualities

This defines the intended material feel, not a material inventory.

- **Mostly matte**. Large surfaces do not shine; reserve reflections for puddles, metal edges and glass.
- **Vary roughness**. Dry and wet patches on one floor are a major source of visual richness.
- **Clean and used, not ruined**. This is an operating film set, not a ruin.
- **Wear has a cause**: only where feet, hands or carts can reach.
- **Visible edges are not knife edges**: use bevels or edge highlights.
- **Objects sit on the ground**: contact shadows at the ground junction.

## 7. Characters

- **No faces.** The player sees only their hands; doubles are afterimages; guards are silhouettes.
- **Player**: dark matte sleeves and hands, with no mechanic colors on the body.
- **Doubles**: film afterimages. Dark, slightly transparent bodies with bright rims in their track color; film-like staccato motion distinguishes them from live people. Since they are solid (can block people and be stood on), they cannot be too transparent.
- **Guards**: completely black matte silhouettes; the only bright element is a red strip across the eyes. Its brightness communicates state.

## 8. Effects and image states

All effects are **optical or film phenomena**, not magic.

| Use | Do not use |
|---|---|
| Dust, lens spots, light beams | Sparks, stars, glowing particle dots |
| Viewfinder lines, corner marks | Energy shields, runes, hexagonal grids |
| Frame lines, one-frame white flashes, hard cuts | Bouncing, trails, colored gradients |
| Fading, bleaching | Persistent glitch-art effects |

Image treatment by state:

| State | Image |
|---|---|
| Walking | Baseline |
| Recording | Slight tightening; red viewfinder corners |
| Inside slow-motion range | Very faint ice-blue rim around the image |
| Rewind | Desaturation and horizontal distortion outside key-light beams; full color and complete stillness inside |
| NG | Hard-cut freeze, whole image bleached silver-gray except the lethal object's red |
| Editing desk | Overhead view, blur at top/bottom, one stop darker, like a miniature tabletop model |

The rewind moment is the most important image in the whole film: communicate “light can lock time” without any text.

## 9. Lens and grading

Aim for a film-print look, not a stylized filter.

| Item | Direction |
|---|---|
| Shadows | Dense but detailed, slightly cool cyan, not pure black |
| Midtones | Neutral |
| Highlights | Gentle rolloff; no large blown-out patches around fixtures |
| Bloom | Small and tight, only on fixtures and emissive mechanics |
| Grain | Extremely light |
| Vignette, lens distortion, lens dirt | None |
| Chromatic aberration | Normally none; only at the instant of rewind |

Do not use global teal-orange separation, a global purple cast, or bloom spilling into fog.

## 10. Unifying reused assets

We will use many ready-made assets from Fab and elsewhere with different styles. Bring them into one image through these rules:

1. **Preserve materials**: retain scanned base-color variation and correct scale; coordinate color temperature by room, without turning everything gray.
2. **Physical character**: adjust roughness separately for bare metal, paint, wood and glass. Do not overstrengthen normals or stretch textures along a raised bridge deck.
3. **Remove text and logos**: remove or cover all existing text, trademarks and branding.
4. **Separate mechanics**: decoration must not use emissive mechanic markers with the same form and brightness; ordinary equipment paint may remain.
5. **Finish through lighting and grading**: shared cool skylight, neutral-white fixtures and unified grading are the most effective way to make different assets belong to one world.

Asset priorities: clear silhouettes and clean forms. Avoid overly detailed assets and strongly cartoon, sci-fi or fantasy styling.

## 11. Acceptance criteria for “AAA and refined”

Check every public-facing screenshot:

1. Subjects, paths and hazards remain clear when desaturated.
2. Foreground, middle ground and background exist; farther elements are paler.
3. Materials can be named and have roughness variation.
4. Objects are grounded, without floating or intersections.
5. No knife-edge right angles.
6. Adjacent surfaces have consistent texture resolution.
7. Detail has dense and sparse areas, with breathing room.
8. Emissive objects are few and unambiguous.
9. All text is clear, aligned and free of corrupted characters.

## 12. Avoiding the AI look

| AI-looking feature | Our rule |
|---|---|
| Teal-orange clashes, purple-cyan neon, colored gradients | Preserve natural material colors; avoid uniform filters and global color separation |
| Glow, outlines and bloom spill everywhere | Only lights and mechanics emit; bloom stays small and tight |
| Plastic appearance, excessive smoothness | Mostly matte, varied roughness |
| Many details without hierarchy | Concentrate detail at focal points and leave the rest quiet |
| Every object has a different style | Unify color and material qualities using §10 |
| Garbled texture text and fake logos | No text in generated images; typeset it separately using our fonts |
| Unmotivated grime and scratches | Wear appears only where contact is possible |

Generated 3D assets are only for small and medium props, not character faces or objects containing text. Apply §10 before importing them into the game too.

## 13. Left to level design

This document does not determine:

- Each stage's exact time of day (within §5).
- Architecture, spatial construction and which ready-made assets to use.
- The specific appearance of mechanisms and props (provided they follow mechanic colors and §6).
- Environmental text, signs and dressing.
- Distant scenery and skyline content.
