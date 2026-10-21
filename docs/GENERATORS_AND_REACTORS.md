# Generators and Reactors

## Goals
- Provide progressive power generation from early to late game.
- Support simple generators and complex multiblock reactors.
- Integrate with energy networks, heat, chemicals, and radiation.

## Simple Generators
- Heat Generator, Solar Generator, Wind Generator, Gas-Burning Generator.
- Tiered output and fuel efficiency.
- Side configuration for energy output.
- Data-driven rates.

## Industrial Turbine
- Multiblock with rotor, blades, coils, condensers.
- Steam input → energy output.
- Efficiency based on size and component quality.
- Formation rules and failure modes (imbalance, overspeed).

## Fission Reactor
- Fuel assemblies, control rods, coolant ports.
- Heat generation and radiation byproduct.
- Cooling loop requirements.
- Meltdown failure mode with clear player feedback.

## Fusion Reactor
- Laser-initiated or injection-based.
- High energy output with coolant and fuel requirements.
- Stability mechanics.
- Integration with heat and energy networks.

## Shared Patterns
- All generators use the energy system.
- Multiblock reactors use the formation and invalidation framework.
- Debug tool reports output, efficiency, and failure reasons.
- Clear progression from simple generators to reactors.
