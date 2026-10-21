# Public API Surface Summary

Stable interfaces intended for addons and internal modules:

## Energy
- IEnergyContainer
- IEnergyHandler
- Energy formatting helpers

## Chemicals
- ChemicalStack
- IChemicalTank
- IChemicalHandler

## Fluids
- IFluidTank
- IFluidHandler

## Side Configuration
- Relative side mode API
- Configurator interaction hooks

## Multiblock
- MultiblockData read access
- Formation status queries

## Gear
- Module registration
- Capability hooks

## Notes
- Document contracts and expected behavior for each interface.
- Prefer interfaces over concrete classes.
- Version public API carefully.
- Internal implementation details should remain private.
