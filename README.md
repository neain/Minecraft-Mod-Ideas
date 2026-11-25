# Minecraft Mod Ideas

A collection of Minecraft mod ideas and experiments that may eventually become actual projects.

These are **idea-stage projects**, not active development projects. Ideas may be explored extensively without necessarily being implemented.

## Ideas

### 1. Physical Inventory

**Status:** Idea / Exploration

A Minecraft inventory overhaul based around the idea that the player should not have a large abstract inventory. Instead, items must be carried in physical containers.

#### Core Concept

Remove the standard player inventory and leave the player with:

* 9-slot hotbar
* Offhand slot
* Existing armor slots
* A dedicated backpack slot
* Additional equipment slots provided through Curios, if appropriate

The hotbar represents the items immediately accessible to the player rather than a collection of arbitrary inventory space.

#### Backpacks

Backpacks occupy a dedicated backpack slot and provide additional storage.

Potential progression:

* Leather backpack
* Iron-upgraded backpack
* Gold-upgraded backpack
* Diamond-upgraded backpack
* Netherite-upgraded backpack

Backpack upgrades may use the vanilla smithing table.

An existing backpack mod such as **Sophisticated Backpacks** may be used rather than implementing a backpack system from scratch.

The actual backpack capacities should be determined through gameplay rather than fixed during the initial design.

#### Equipment

Armor slots remain unchanged.

Additional equipment slots may be provided through **Curios**, potentially allowing things such as:

* Backpack
* Rings
* Amulets
* Other wearable equipment

No additional generic "pocket" slots are planned. The 9-slot hotbar is considered sufficient immediate carrying space.

#### Physical Storage

The mod should encourage physical storage and transportation rather than simply recreating the vanilla inventory in another form.

Potential storage/transport objects include:

* Wheelbarrows
* Carts
* Larger carts
* Chest-equipped carts
* Other physical containers

The goal is for additional carrying capacity to exist **in the world**, rather than being abstracted into player inventory slots.

---

### 2. Primitive Railways / Physical Transportation

**Status:** Idea / Exploration

A transportation system designed around inexpensive, primitive infrastructure that can eventually be upgraded into conventional iron railways.

This may eventually be part of the Physical Inventory mod or developed as a separate mod.

#### Stone Track

Stone blocks in the world can be carved into primitive rail tracks.

The track remains approximately a full-sized stone block rather than becoming a thin rail placed on top of the world.

Possible implementation:

* Carve an existing stone block into a stone track.
* A craftable stone-track item should also exist so that inconvenient blocks such as gravel do not prevent construction.
* Normal mining drops cobblestone.
* Silk Touch preserves the specialized track block.

Stone track intentionally has poor momentum retention compared with vanilla iron rails.

The goal is not to prevent slopes or other uses, but simply to make stone track inefficient at maintaining cart speed.

#### Wooden-Reinforced Track

A stone track can be upgraded with a stick.

Possible behavior:

* Remains a mostly full-sized stone block.
* Internally represented as a wooden-reinforced track.
* Normal mining drops cobblestone and a stick.
* Silk Touch preserves the specialized track block.
* Better momentum retention than stone track.
* Still worse than iron rails.

#### Iron Track

A stone/wooden track can be upgraded using an iron nugget.

If the space above the track is clear:

1. The specialized track block becomes a normal stone block.
2. A standard vanilla iron rail is placed on top.
3. The upgrade effectively converts primitive infrastructure into a conventional railway.

Using one iron nugget per segment provides a substantial iron savings compared with crafting vanilla rails directly.

Vanilla:

> 6 iron ingots → 16 rails

Proposed upgrade system:

> 6 iron ingots → 54 upgraded rail segments

The intent is to make large railway networks practical without requiring enormous quantities of iron.

#### Track Speed Progression

The exact values should be determined through testing, but the intended progression is:

* **Stone:** slightly too slow to comfortably pull a cart while walking.
* **Wood:** approximately walking speed and slightly too slow for comfortable sprinting.
* **Iron:** capable of keeping up with a sprinting player.

The goal is for the player to naturally experience the technological progression:

> Stone = "I can use this."

> Wood = "This is practical."

> Iron = "This is fast."

#### Cart Hitching

Instead of creating a completely new coupling system, the mod should attempt to reuse Minecraft's existing lead mechanics.

Potential system:

1. Right-click a minecart with a lead.
2. Right-click a hitch/fence-post attachment on another minecart.
3. The two carts become connected by the lead.
4. Repeat to create a train of carts.

Possible hitch cart:

> Minecart + fence post → minecart with visible hitch

A train could therefore look like:

```text
Player ── lead ── [Cart] ── lead ── [Cart] ── lead ── [Cart]
```

The player's connection to the first cart would use normal lead behavior.

If the player moves faster than the connected train can travel, the existing lead tension/breaking behavior should handle the connection breaking naturally.

#### Transportation Philosophy

The goal is to make transportation an actual physical part of Minecraft rather than simply expanding player inventory.

A player should be able to:

* Build a primitive railway from stone.
* Attach several storage carts.
* Pull the carts manually.
* Upgrade important railway sections with wood or iron.
* Eventually use the railway as physical logistics infrastructure between mines, workshops, storage areas, and bases.

The broader philosophy is:

> **If you want to carry more, build something that can physically carry it.**

## Potential Dependencies

Initial concept assumes:

* **NeoForge**
* **Minecraft 26.1.x**
* **Curios** — equipment-slot infrastructure
* **Sophisticated Backpacks** — potential backpack implementation

These dependencies are tentative and should be reassessed if development ever begins.

---

### 3. Adjustable Block Lighting

Status: Idea / Exploration

A utility mod that allows users to change the light emission levels of existing blocks through a configuration file.

The mod is intended to work alongside Ultrabright Lib, allowing existing light sources to emit light levels beyond Minecraft's normal maximum of 15.

The goal is to make lighting more practical and configurable without introducing additional decorative blocks or requiring users to replace existing light sources.

#### Core Concept

Allow users to specify the desired light emission level of individual blocks.

For example:

- Vanilla torch: 14 → 25
- Vanilla lantern: 15 → 30
- Glowstone: 15 → 35
- Industrial floodlight: configurable to 60 or higher

The changes apply globally to the specified block type rather than individual placed blocks.

Blocks not included in the configuration retain their normal light emission values.

#### Configuration

All lighting overrides should be managed through a configuration file.

Potential format:

```toml
[blocks]

# Vanilla
"minecraft:torch" = 25
"minecraft:wall_torch" = 25
"minecraft:lantern" = 30
"minecraft:wall_lantern" = 30
"minecraft:glowstone" = 35
"minecraft:sea_lantern" = 40

# Modded blocks
"immersiveengineering:example_lamp" = 45
"some_mod:industrial_floodlight" = 60
```

The actual configuration format may change during implementation.

Light levels should support values from 0 to 127, matching Ultrabright Lib's advertised maximum.

A value of 0 disables the block's emitted light.

#### Mod Compatibility

The configuration should support blocks from any installed mod without requiring direct integration with those mods.

Block identifiers should be resolved through Minecraft's block registry rather than hard-coded mod dependencies.

Configuration entries for blocks belonging to mods that are not installed should:

- Be ignored without causing errors.
- Remain in the configuration file.
- Automatically become active if the corresponding mod is installed later.

This allows one configuration file to contain lighting settings for an entire mod collection, including optional mods.

#### Block State Behavior

Some blocks have state-dependent light emission.

For example, a redstone lamp emits light when powered but not when unpowered.

The override system should ideally preserve the original block's state-dependent behavior while changing its maximum or base emission value.

The implementation should avoid unintentionally making normally unlit blocks emit light.

#### World Lighting Updates

Changes to emission values must apply to both newly placed and already existing blocks.

Existing blocks should receive updated lighting when their chunks are loaded, and the resulting light levels must persist correctly after saving and reloading the world.

The implementation should avoid unnecessary full-world lighting recalculations.

#### Performance

The mod should have minimal ongoing performance overhead.

- No continuous block scanning.
- No per-tick configuration processing.
- No unnecessary block entity creation.
- Resolve configured block identifiers during initialization.
- Use efficient lookups when checking emission overrides.

The actual lighting calculations and extended light propagation should be handled by Ultrabright Lib.

#### Dependencies and Compatibility

Initial concept assumes:

- NeoForge
- Ultrabright Lib

The exact Minecraft version must be determined before development.

The mod should not require individual content mods as dependencies.

#### Initial Scope

The initial version should focus exclusively on configuration-based block light overrides.

The following are intentionally excluded:

- In-game commands.
- Configuration GUI.
- Dynamic lighting from held items.
- New light-emitting blocks.
- Changes to light propagation distance or falloff.

The priority is to make existing block light sources configurable and ensure that extended lighting works reliably.

#### Future Possibilities

Potential future additions include:

- Optional configuration presets.
- More advanced handling of state-dependent emission.
- Additional compatibility improvements for other lighting and content mods.

The project should remain a lightweight utility rather than becoming a general lighting overhaul.

## Future Ideas

Additional Minecraft mod concepts can be added as separate sections or folders as they become interesting enough to document.

This repository is primarily a place to **capture ideas before they disappear**, not a commitment to implement every idea.


