# JEI / EMI Integration for Mekanika

This document provides detailed, clean-room design guidance for JEI and EMI integration. All content is original and conceptual.

## Goals

- Clear, informative recipe display for all machines, factories, and multiblocks.
- Support for item, fluid, chemical, and energy inputs/outputs.
- Ghost ingredient dragging into machine slots.
- Multiblock structure previews and material cost information.
- Consistent visual language with in-game GUIs.
- Data-driven recipe registration so balance changes do not require code updates.
- Support for both JEI and EMI where practical.

## Recipe Categories

- One primary recipe category per machine type (Enrichment Chamber, Chemical Injection Chamber, etc.).
- Factory recipes can share the same category or have a dedicated factory view.
- Multiblock structures may have info pages rather than traditional recipes (formation rules, material counts, outputs).
- Categories display processing time, energy cost, and chemical/fluid requirements.

## Display Elements

- Item slots for inputs and outputs.
- Fluid and chemical gauges with type and amount.
- Energy cost indicator.
- Processing time or speed.
- Secondary outputs with chance indicators where applicable.
- Tier or upgrade notes if they affect the recipe.

## Ghost Ingredients

- Support dragging ingredients from JEI/EMI into machine and factory slots.
- Respect slot validation and filters.
- Provide feedback if an ingredient cannot be placed.
- Work for both single machines and factories.

## Multiblock Previews

- Info pages or categories that show:
  - Required frame and internal blocks.
  - Minimum and maximum size where applicable.
  - Material counts.
  - Special rules (controller placement, ports, valves).
  - Expected outputs or rates.
- Avoid hard-coding layouts; derive counts from validator rules or data where possible.
- Include tier and upgrade information where relevant.

## Data-Driven Registration

- Recipes are registered from data packs or data generators.
- Chemical and fluid types, amounts, and energy costs come from the same data used by machines.
- Support for data pack overrides.
- Document the recipe format so design notes map cleanly.

## Implementation Recommendations

1. Define recipe categories and display widgets early.
2. Integrate with the recipe and data generation system.
3. Add ghost ingredient support once machine GUIs are stable.
4. Create multiblock info pages after structure validators are defined.
5. Support both JEI and EMI if feasible, or prioritize one with a clear path for the other.
6. Rate-limit or cache recipe lookups for performance.
7. Provide clear tooltips for chemical and fluid amounts.

## Edge Cases

- Dynamic recipes based on upgrades or tier.
- Multiblock recipes with variable outputs or sizes.
- Chemical attribute-based recipes.
- Factories with multiple slots processing the same recipe.
- Missing chemical or fluid types in data.
- Large numbers of recipes and performance.

## Player Feedback

- Clear display of all inputs, outputs, and costs.
- Tooltips with exact amounts and types.
- Multiblock pages that help players plan structures.
- Consistent with in-game GUI visual language.

## Integration Points

- Recipe and data generation system.
- Machine and factory GUIs for ghost ingredients.
- Multiblock validators for structure requirements.
- Balance workflow for rates and ratios.
- Debug tool can reference recipe data.

This design ensures players can understand and plan their setups using JEI or EMI while keeping the integration maintainable and data-driven.
