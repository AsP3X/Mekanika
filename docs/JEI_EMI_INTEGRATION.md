# JEI / EMI Integration

## Goals
- Clear recipe display for all machines and multiblocks.
- Multiblock previews and material cost information.
- Ghost ingredient support.

## Patterns
- One recipe category per machine type.
- Chemical and fluid inputs/outputs shown with gauges.
- Multiblock info pages listing required blocks and formation rules.
- Transfer handlers for dragging ingredients into slots.

## Implementation Notes
- Data-driven recipe registration.
- Claude provides structure blueprints for previews.
- Support both JEI and EMI where practical.
- Document recipe builders in the public API if addons need them.

## Edge Cases
- Dynamic recipes based on upgrades or tier.
- Multiblock recipes with variable outputs.
- Chemical attribute-based recipes.
