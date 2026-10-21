# Recipe and Data Generation System for Mekanika

This document provides detailed, clean-room design guidance for recipes, tags, and data-driven values. All content is original and conceptual.

## Goals

- Keep machine behavior, ratios, and costs data-driven.
- Allow balance changes without code edits.
- Make Claude’s balance notes map directly to data files.
- Support JEI/EMI display of all recipes.
- Separate recipe logic from tile logic.

## Recipe Types

- Item-to-item (enrichment, crushing, smelting).
- Item + chemical → item (purification, injection).
- Item + fluid → item or chemical.
- Chemical combination (infuser).
- Dissolution, washing, crystallization chains.
- Factory recipes reuse the same handlers as base machines.

## Data Structure

- Recipes stored as JSON in data packs.
- Each recipe specifies:
  - Input items, fluids, chemicals (with amounts).
  - Output items, fluids, chemicals.
  - Energy cost and base processing time.
  - Optional tier or upgrade modifiers.
- Tags used for ore dictionary style inputs (e.g., c:ingots/iron).
- Tier multipliers (speed, energy, capacity) stored in config or data files.

## Data Generation

- Use data generators for repetitive recipes (ore processing variants, tiered items).
- Generators should be maintainable and clearly map to design notes.
- Output both recipes and tags.
- Support overrides via data packs for modpack authors.

## Integration

- Machines look up recipes via a shared recipe manager.
- Factories use the same recipe lookup per slot.
- JEI/EMI plugins read the same recipe data.
- Chemical and fluid amounts use the same units as their systems (mB, long).

## Balance Mapping

- Document config/data keys for:
  - Processing time and energy per machine.
  - Chemical consumption rates.
  - Tier multipliers.
  - Ore multiplication ratios.
- Provide a simple spreadsheet or markdown table that maps design notes to keys.

## Implementation Recommendations

1. Define recipe interfaces early.
2. Implement data generation for core ore processing chains first.
3. Keep recipe logic separate from tile ticking.
4. Add JEI integration once recipes exist.
5. Make all numeric values data-driven from the start.

## Edge Cases

- Missing recipes or invalid data.
- Conflicting data pack overrides.
- Very large recipe counts and lookup performance.
- Factory slots with different valid recipes.

This system ensures balance and content can evolve without repeated code changes.
