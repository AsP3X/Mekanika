# Recipe and Data Generation System

## Goals
- All recipes, tags, tier multipliers, and ratios are data-driven.
- Claude’s balance notes map directly to data files or configs.
- Easy to add or adjust recipes without code changes.

## Structure
- Recipe types for each machine (enrichment, crushing, chemical infusion, etc.).
- Tiered recipes or recipe multipliers where appropriate.
- Tags for ore processing compatibility.
- Data generators for recipes, tags, and loot tables.

## Implementation Notes
- Use Minecraft’s data gen system.
- Keep recipe builders in the public API where possible.
- Document the expected data format for each recipe type.
- Support both JSON and data gen for flexibility.
