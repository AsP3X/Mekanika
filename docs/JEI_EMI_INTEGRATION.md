# JEI / EMI Integration — Clean-Room Design

This document defines the conceptual design and implementation guidance for JEI and EMI integration in Mekanika. All content is original.

## Goals

- Provide clear, consistent recipe displays for all machines, factories, and multiblock structures.
- Show energy, chemical, and fluid requirements with visual gauges.
- Support ghost ingredient dragging into machine slots.
- Offer multiblock previews and material cost summaries.
- Support both JEI and EMI where practical.
- Keep registration data-driven so balance changes do not require code edits.

## Recipe Categories

- One category per major machine type (Enrichment Chamber, Crusher, Chemical Infuser, etc.).
- Factories share the base machine category or use a shared factory category with slot count noted.
- Multiblock structures get info pages rather than traditional recipes where appropriate.
- Chemical and fluid recipes display input/output gauges with amounts and rates.
- Energy cost is shown as a rate (J/t) or total cost depending on the machine.

## Display Patterns

### Machine Recipes
- Input slots on the left, output slots on the right.
- Progress arrow or bar in the center.
- Energy gauge or rate label.
- Chemical and fluid gauges near relevant slots.
- Tooltip with exact amounts, rates, and upgrade effects if applicable.

### Multiblock Info Pages
- Structure name and description.
- Required blocks and minimum size.
- Formation rules summary.
- Material cost list.
- Optional 3D or schematic preview (provided by Claude).
- Links to related machines or chemicals.

### Chemical / Fluid Recipes
- Use consistent gauge widgets.
- Show both amount and rate where relevant.
- Support attribute-based recipes (e.g., any chemical with a certain property).

## Ghost Ingredient Support

- Transfer handlers allow dragging ingredients from JEI/EMI into machine and factory slots.
- Support for chemical and fluid ghost ingredients where the API allows.
- Clear visual feedback when a ghost ingredient cannot be placed.
- Compatible with the side configuration and slot validation rules.

## Implementation Recommendations

1. Register recipes through a data-driven system so Claude can adjust balance without code changes.
2. Create reusable recipe display widgets for energy, chemicals, and fluids.
3. Implement transfer handlers for item, chemical, and fluid slots.
4. Provide multiblock info pages with material lists and formation summaries.
5. Support both JEI and EMI; prefer shared abstractions where possible.
6. Document recipe builder helpers if addons need to register custom recipes.
7. Rate-limit or cache heavy recipe lookups.
8. Test ghost ingredient dragging with factories and multiblock controllers.

## Edge Cases

- Dynamic recipes that change with upgrades or tier.
- Multiblock recipes with variable outputs based on structure size.
- Chemical attribute-based recipes.
- Recipes that consume or produce radiation or heat.
- Factories with multiple slots sharing the same recipe.
- Missing JEI/EMI at runtime (graceful degradation).
- Large recipe counts affecting search performance.

## Player Feedback

- Clear tooltips for all inputs, outputs, and rates.
- Visual distinction between item, fluid, and chemical ingredients.
- Formation requirements shown before the player attempts to build.
- Error messages if a ghost ingredient cannot be placed.

## Notes

- Claude provides structure blueprints and material lists for multiblock previews.
- Grok implements the registration, display, and transfer handlers.
- Keep the public API surface for recipe builders small and stable.

Strong JEI/EMI integration improves discoverability and reduces the need for external guides.
