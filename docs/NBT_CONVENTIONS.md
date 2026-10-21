# NBT and Data Component Conventions

## General Rules
- Prefer data components for items where available.
- Use clear, versioned keys for tile NBT.
- Save only what cannot be recalculated.

## Energy
- Save current amount and capacity if variable.

## Chemicals / Fluids
- Save type and amount.

## Side Configuration
- Save full configuration map.

## Multiblocks
- Save controller position, aggregated stats, and cache data.
- Recalculate locations on load.

## Gear
- Save installed modules and energy in item data.

## Versioning
- Include a version key for future migrations.
