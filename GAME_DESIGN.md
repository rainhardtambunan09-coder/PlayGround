# FAIRESTATLE — Game Design & Technical Blueprint

**Version:** Prototype 0.1 · **Genre:** third-person open-world survival, crafting & exploration · **Target:** PC first, touch-friendly web prototype

## High concept

A violent storm tears apart a passenger aircraft over Fairestatle, a sprawling, uncharted tropical archipelago. Washed onto a bright, deceptively calm shore, the survivor turns wreckage into shelter, learns the rhythms of a living island and follows three distant beacon lights toward the people who may have survived before them. The emotional arc moves from **shock → foothold → curiosity → self-reliance → discovery**. Survival is a pressure system that creates choices, not a punishment treadmill; exploration is the reward for mastering a place.

**Player promise:** Every trip out is a small expedition. The coastline is a readable home base; the reef and distant islands are increasingly risky, increasingly rewarding routes to the mystery.

## Design pillars

1. **A tactile, legible wilderness.** Organic, non-voxel landforms, recognizable resource silhouettes, warm PBR materials, dense but navigable foliage and clear interaction language.
2. **Make the island work for you.** Gather, process, craft, cook, build, and carry the result physically into the world. Tools change what the player can harvest; structures change the safety and range of expeditions.
3. **The sea is a world, not a border.** Shallows invite wading and reef gathering; deeper water introduces oxygen, predators, wrecks, and boat travel.
4. **Wildlife has readable intent.** Animals communicate through posture, sound, and distance. Their behaviors are consistent enough to learn and varied enough to surprise.
5. **A mystery worth surviving for.** Survivor notes, wreck sites, and the beacon chain turn the open world into a sequence of discoveries rather than a checklist.

## Opening sequence and onboarding

- **Storm / aircraft (target production sequence):** First-person seat view, restrained camera shake, lightning flashes, cabin alarms and short environmental lines. A tearing-fuselage beat cuts to white/black with muffled audio; avoid long unskippable exposition.
- **Shore transition:** Sound returns under surf. Camera rises from water into a third-person follow shot. Burning wreckage, floating luggage, and debris form the tutorial landmark and first resource field.
- **Playable tutorial:** One contextual task at a time: salvage wood and stone → craft a stone axe → fell a palm → build a campfire → drink/eat → read the note → make a raft and leave the lagoon.
- **Prototype adaptation:** The browser prototype uses a title card as the skip-friendly cinematic abstraction, then starts directly on the wreck beach. Wreckage, salvage, journal clue, controls, survival HUD, gathering, crafting, swimming, wildlife and skiff crafting are implemented as playable vertical-slice systems.

## Core gameplay loop

**Observe** the shoreline / weather / wildlife → **plan** for needs and destination → **travel** on foot, by swim, or raft → **gather** with the correct tool → **manage** hunger, thirst, stamina, temperature, fatigue and oxygen → **craft/build/cook** → **recover and record clues** → **push farther** to the next island or reef.

### Minute-to-minute loop

- Scavenge plane debris, driftwood, stones, coconuts and vines.
- Craft a crude axe and other tools; upgraded tools increase yield and unlock hardwood, mineral seams and underwater resources.
- Cut, carry and convert materials into useful structures. Placement is snap-assisted but can be nudged freely; physics settles pieces after placement.
- Use campfires for warmth, cooking and morale. Prepare food/water before long trips.
- Choose whether to explore the reef or return to base as oxygen, thirst, weather and daylight change.
- Read survivor clues that reveal routes, caches and the beacon mystery.

### Long-term progression

**Wreck beach → sheltered camp → dependable food and water → coastal raft → reef / shipwrecks → neighboring island outposts → deep ocean gear and storm navigation → beacon chain → story resolution.** Each island introduces a material, hazard, wildlife behavior, and piece of the mystery; progression is gated by capability and knowledge, not invisible level scaling.

## Player controller and camera

- Third-person over-the-shoulder with smoothed follow, collision-aware spring arm, mouse/touch drag look, camera-relative movement, acceleration/deceleration, grounded jump, sprint drain, and coyote-time tuning.
- FOV eases wider during sprint/swim; the camera lowers and adds drag under water. Optional aim / tool focus narrows FOV.
- State machine: `Grounded`, `Airborne`, `SwimmingSurface`, `Diving`, `Climbing`, `Rafting`, `Stunned`, `Exhausted`.
- Context actions resolve from a short forward interaction query and prioritize interactables with clear prompts; an axe is required to chop, a pick is required to mine. Interaction must never silently consume a scarce resource.
- Stats: health, stamina, hunger, thirst, body temperature, sanity/fatigue, oxygen. Drain rates are low and forecastable. Exposure and exhaustion affect recovery / aim rather than instantly killing the player. A production build supports configurable accessibility rates and HUD scale.

## Survival, crafting and building

### Resource economy

| Source | Base yield | Tool / risk | Useful for |
|---|---:|---|---|
| Wreckage / driftwood | 1–2 timber | Free; finite beach supply | Starter tools, fire, raft |
| Palm | timber + fronds / coconuts | Stone axe; tree-fall zone | Shelter, water, raft |
| Hardwood | hardwood + bark | Improved axe; slower harvest | Durable frames, boat |
| Stone outcrop | stone + flint | Stone pick | Tools, fire ring, spear tip |
| Vine / fiber plants | cordage | Free / knife improves yield | Lashing, traps, structures |
| Reef kelp / shell bed | kelp, shell, pearl chance | Dive / oxygen budget | Food, medicine, trade/story |
| Fish / crab / boar | meat, hide, bone | Spear, trap or hunting risk | Cooked meals, gear |

Trees use a staged health / damage model; at zero health the trunk falls as a physical object, splits at authored break joints into log pickups, and leaves a regrowth stump/timer. Rocks fracture into authored chunks with deterministic drop counts. Keep harvesting clear and non-destructive to frame rate with pooled debris and a low cap on live rigid bodies.

### Starter crafting tree

```text
Wreckage + beach stones
  └─ Stone axe (2 driftwood + 2 stone)
      ├─ Palm logs / fronds + vines
      │   ├─ Lean-to (4 timber + 4 fronds + 2 cordage)
      │   ├─ Storage chest (3 timber + 2 cordage)
      │   └─ Reef skiff (6 timber + 3 cordage)
      └─ Stones + driftwood
          ├─ Campfire (3 timber + 1 stone)
          └─ Stone spear (2 timber + 1 flint + 1 cordage)

Vine plants ── cordage ── fish trap / water still / raft lashings
Coconut ── drink / food; cooked fish + kelp ── restorative meal
Reef shell + kelp + pearl ── goggles → fins → oxygen tank
Hardwood + hide + cordage ── reinforced boat / elevated hut
```

### Building

Production placement combines snap sockets for structural integrity with freeform offset/rotation inside a placement preview. Pieces validate support, slope, overlap and material cost before commit. Foundations, posts, beams, roof panels, woven walls, ladders, doors and utility stations are separate modules. Rafts are modular buoyant assemblies with sail / paddle controls and a lightweight stability model; no voxel terrain or block construction.

## Marine ecosystem and underwater play

- Layer habitats by depth and temperature: lagoon reef, seagrass flat, shelf edge, blue-water drop-off, wreck interior.
- Common fish school and flee; turtles cruise and surface; rays glide along the sand; eels defend crevices; octopuses camouflage and escape; reef sharks investigate blood/noise then disengage; night jellyfish glow and apply a mild sting.
- Swimming supports full 3D movement, surface/dive transition, buoyancy, current zones, visibility falloff, pressure depth, oxygen consumption and surface recovery. Dive plan: identify target → budget outbound/return oxygen → harvest or explore → abort early when weather/current changes.
- Spearfishing uses a short aim window and a projectile / hit zone. Kelp and pearl nodes have readable harvest states. Wrecks hold salvage, story clues and navigation hazards.
- Crafting path: goggles (visibility) → fins (efficiency) → oxygen tank (depth/range). Never require a tank for the first reef tutorial.

## Wildlife state machines

### Shared perception and state set

`Sleep / Idle → Wander → Notice → Investigate → Flee | Defend | Steal → Cooldown → Idle`.
Use species-specific sight/hearing/scent radii, memory timers and breakable pursuit. Readable anim/audio cue on state transitions; never use omniscient pursuit through walls.

| Species | Signature states / decisions |
|---|---|
| Wild boar | Graze → scent/notice player → warning posture/snort → short territorial charge → lose interest after distance/time. Avoidance, spear threat and group proximity influence aggression. |
| Monkeys | Canopy forage → curious approach → inspect dropped food → steal a loose item → flee/climb. Offering fruit builds trust; later monkeys can reveal caches. |
| Crabs | Scuttle along tide line → hide under rock / retreat from shadow → forage. Can be trapped or harvested. |
| Tropical birds | Perch / flock / startle; flight direction can hint at water or nearby danger. |
| Nocturnal predators | Sleep in daylight → patrol at dusk → stalk from cover → attack only when isolated / unlit → break off at torch, fire or shelter. |
| Marine life | School / graze / cruise → detect player → flee / investigate / defend habitat; species are non-hostile unless provoked except designated predators. |

## World, art and audio direction

- Semi-realistic stylized PBR: broad organic forms and carefully chosen silhouettes; physically plausible roughness/metalness with painterly color grouping. Never voxel/block terrain.
- Terrain: hand-authored heightfields and sculpted coastlines, erosion breakup, layered sand/soil/grass/scree materials, instanced grass and foliage with distance / wind LODs.
- Lighting: warm tropical sun, golden-hour exposure, cloud shadows, volumetric storm shafts/fog, lightning with controlled exposure, night readability via fire/torch pools and bioluminescence. Weather changes gameplay gradually: rain cools the player, storms reduce visibility and make offshore sailing dangerous.
- Water: shallow/deep color blend, depth fade, animated normal maps, shoreline foam, caustic decals / projected texture, underwater post-process and light shafts. Ocean shader is depth-aware and bounded by performance tiers.
- Sound: layered shore wash, directional bird/insect beds, storm low-frequency pressure, material-specific harvest impacts, creature vocal signatures, low-key adaptive score. The aircraft opening filters sound through cabin metal; the first shore reveal opens the stereo field.

## Technical architecture (Unity production target)

**Engine assumption:** Unity 2022 LTS / Unity 6, URP for broad hardware reach; HDRP is a high-end option. The playable vertical slice in this repository is a dependency-light browser implementation in Three.js; its controls/systems are deliberately mapped to the Unity architecture below.

### Runtime layers

- `WorldBootstrap`: seed, biome chunks, weather/time-of-day, save/load, world service registry.
- `Player`: `CharacterController` motor, camera rig, input adapter, `PlayerStateMachine`, `SurvivalVitals`, `Inventory`, `InteractionScanner`.
- `Interactables`: `IInteractable` interface (`Prompt`, `CanInteract`, `Interact`), `HarvestNode`, `LootContainer`, `ReadableClue`, `CraftingStation`.
- `Items & crafting`: ScriptableObject item/recipe definitions; inventory transactions validate and consume atomically; events update UI/audio/save.
- `Building`: preview ghost, socket/surface queries, placement validation, modular `BuildPiece` definitions, pooled physics release.
- `AI`: data-driven species config + perception, state machine, navigation, animation/audio event adapters; simulation LOD by distance.
- `Environment`: biome streamer, foliage/rock scatter, water volumes, weather controller, day/night and audio zones.
- `Persistence`: versioned save DTOs (player, inventory, constructed objects, node depletion/regrowth, story flags, world seed), resilient migration and autosave checkpoints.

### Data / code boundaries

- Input is abstracted (`Move`, `Look`, `Jump`, `Sprint`, `Interact`, `Craft`, `Dive`) so keyboard/gamepad/touch do not leak into systems.
- Systems communicate via typed events (`ItemAdded`, `NodeHarvested`, `VitalsChanged`, `ClueFound`, `WeatherChanged`) rather than UI references.
- Authoritative game state is simulation data; scene objects are views. Resource quantities and recipe costs are data assets. Use deterministic world seed and stable object IDs to make saves robust.
- Performance budgets: 60 fps target on midrange PC, 30 fps on low-tier mobile; chunked LOD, GPU-instanced foliage, simplified distant AI, pooled impact debris, capped shadow casters, async scene / asset loading.
- Networking is explicitly out of prototype scope; multiplayer would require server-authoritative inventory, placement, harvest ownership and AI replication.

### Production hardening / prototype gaps

The web prototype uses primitive procedural geometry, one island terrain mesh plus nearby island silhouettes, simplified survival stats and placeholder fauna. It demonstrates the playable loop; it is not a full AAA asset set or production physics simulation. Production requires authored cutscene / animation, real mesh/material assets, robust collision/grounding, proper building-placement preview, save/load, sound, accessibility, platform QA, and extensive balance/bug testing.
