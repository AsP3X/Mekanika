# Induction Matrix

Dimensions, block ids, tier defaults, and the transfer budget are specified in [MULTIBLOCK_STRUCTURES.md](MULTIBLOCK_STRUCTURES.md).

## Goals
- Large-scale energy storage with high throughput.
- Separation of capacity (cells) and transfer rate (providers).

## Design
- Multiblock structure with cells and providers.
- Ports for network connection.
- Aggregated capacity and rate calculated at formation.
- Mixed-tier support.
- Controller owns the energy value.

## Behavior
- High capacity but throughput limited by providers.
- Side configuration on ports.
- Debug tool reports total capacity, rate, and fill level.

## Notes
- Meaningful upgrade over stacks of Energy Cubes.
- Data-driven cell and provider values.
