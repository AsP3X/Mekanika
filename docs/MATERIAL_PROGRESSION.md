# Material Progression for Mekanika

This document defines early-game materials, alloys, and tier progression. All content is original and conceptual.

## Goals

- Provide a clear path from basic ores to advanced alloys.
- Support four tiers: Basic, Advanced, Elite, Ultimate.
- Integrate with ore processing and factories.
- Keep all recipes data-driven.
- Use original names while preserving a familiar industrial progression.

## Core Materials

- Primary metal (osmium-style role): ore, raw ore, ingot, dust, nugget.
- Supporting materials: use vanilla or common tags (iron, copper, tin, gold, redstone, etc.) where possible.
- Alloys (original names):
  - Infused Alloy (Basic → Advanced bridge)
  - Reinforced Alloy (Advanced → Elite)
  - Atomic Alloy (Elite → Ultimate)
- Control circuits at multiple tiers.
- Energy tablets and other early components.

## Tier Progression

- **Basic**: Early machines, basic cables, energy cubes, enrichment.
- **Advanced**: Improved rates, chemical processing begins, infused alloys.
- **Elite**: Factories, reinforced alloys, better storage, advanced transmitters.
- **Ultimate**: Top-tier machines, reactors, QIO, atomic alloys.

## Machine Upgrading

- Tier installer items convert a machine to the next tier.
- Preserves contents, side configuration, upgrades, and redstone settings where possible.
- Recipes are data-driven.
- Visual change via Claude-provided textures.
- Factories upgrade slot count with tier.

## Integration

- Feeds into the ore processing chain.
- Alloys used in machine, cable, and circuit crafting.
- JEI displays the full progression and alloy recipes.
- Config can adjust alloy ratios if desired.

## Implementation Recommendations

1. Register ores, items, and tags early.
2. Use data generators for repetitive tiered recipes.
3. Document default crafting ratios.
4. Provide lang keys for all names.
5. Test the full Basic → Ultimate path.
6. Ensure tier installers work on both single machines and factories.

## Edge Cases

- Upgrading a machine with items inside.
- Missing intermediate alloys.
- Creative mode infinite materials.
- Data pack overrides of alloy recipes.

Clear material progression makes the mod approachable while supporting deep late-game systems.
