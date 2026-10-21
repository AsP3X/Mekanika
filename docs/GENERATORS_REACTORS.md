# Generators and Reactors for Mekanika

This document provides detailed, clean-room design guidance for power generation systems. All content is original and conceptual.

## Goals

- Clear progression from early generators to advanced reactors.
- Integration with energy, heat, chemical, and fluid systems.
- Documented fuel, coolant, and failure modes.
- Data-driven rates and capacities.

## Early Generators

- Heat Generator, Solar Generator, Wind Generator, Bio Generator, Gas-Burning Generator.
- Simple energy output based on fuel or environmental conditions.
- Tiered where applicable.
- Integration with energy networks and side configuration.

## Industrial Turbine

- Multiblock that converts steam to energy.
- Requires blades, coils, and vents.
- Aggregates blade and coil contributions at formation.
- Heat and fluid integration.
- Failure modes for insufficient steam or structural issues.

## Fission Reactor

- Multiblock that consumes fissile fuel and coolant.
- Produces heat, waste, and energy (via turbine or direct).
- Fuel assemblies and control rods.
- Radiation and heat management.
- Clear critical failure modes for insufficient cooling.

## Fusion Reactor

- Advanced multiblock requiring specific structure and energy input to start.
- Consumes deuterium/tritium or equivalent fuel.
- High energy output.
- Laser or ignition requirements.
- Strict formation and safety rules.

## Integration Points

- Energy system for output.
- Heat system for boilers and cooling.
- Chemical and fluid systems for fuel and coolant.
- Multiblock framework for formation and invalidation.
- Radiation system for fission waste.
- Debug tool for status reporting.

## Implementation Recommendations

1. Implement early generators after the energy system is stable.
2. Add turbine and boiler after multiblock and heat systems.
3. Implement fission after radiation and chemical systems.
4. Fusion as a late-game milestone.
5. Make all rates and fuel requirements data-driven.
6. Document failure modes clearly.

## Edge Cases

- Insufficient coolant or steam.
- Rapid formation and invalidation.
- Mixed-tier internals.
- Chunk unload while active.
- Creative fuel sources.

This design supports a clear power progression while integrating with existing systems.
