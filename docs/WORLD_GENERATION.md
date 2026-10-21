# World Generation for Mekanika

This document defines ore and resource generation. All content is original and conceptual.

## Goals

- Provide a clear early-game resource path.
- Keep generation data-driven and configurable.
- Support Silk Touch and Fortune.
- Remain compatible with other mods via tags.
- Avoid aggressive world changes by default.

## Core Ore

- Primary ore (osmium-style role): generates in the overworld.
- Configurable vein size, veins per chunk, and Y-range.
- Default values documented and overridable in config.
- Drops raw ore; Silk Touch yields the block; Fortune increases drops.
- Tagged under a common tag (e.g., `c:ores/osmium_style`) for compatibility.

## Supporting Resources

- Optional salt or secondary resources if needed for chemical chains.
- Generation can be disabled per resource.
- No forced biome restrictions by default; optional biome filters via config.

## Configuration Keys (Examples)

- `worldgen.ore.enabled` (boolean, default true)
- `worldgen.ore.veinSize` (int, default 8)
- `worldgen.ore.veinsPerChunk` (int, default 6)
- `worldgen.ore.minY` / `maxY` (ints)
- `worldgen.ore.fortuneMultiplier` (float)

## Implementation Notes

- Use configured features or data-driven generation.
- Register features early in the mod lifecycle.
- Test chunk generation performance with default and high values.
- Provide a sample config section with comments.
- Document regeneration behavior on existing worlds (usually new chunks only).

## Edge Cases

- Disabled generation.
- Extreme vein sizes.
- Conflicts with other mods’ ore generation.
- Creative or flat worlds.
- Dimension-specific overrides.

## Player Feedback

- Clear ore texture and name.
- JEI shows the ore and its drops.
- Lang keys for all names.

This system supplies the base materials for Mekanika’s progression without requiring complex world changes.
