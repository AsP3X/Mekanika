# Upgrade System

## Goals
- Shared, data-driven upgrade component for machines and factories.
- Clear effects on speed, energy, gas, and other stats.
- Configurable stack limits.

## Upgrade Types
- Speed: Increases processing rate.
- Energy: Reduces energy consumption.
- Gas: Reduces chemical/fluid consumption.
- Muffling: Reduces sound.
- Filter: Improves filtering on transporters.
- Anchor: Prevents chunk unloading for the machine.

## Behavior
- Upgrades are stored in a dedicated component or attachment.
- Effects are calculated from base values + upgrade count and multipliers.
- Stack limits are data-driven per machine type.
- Factories apply upgrades to all slots.

## Integration
- Config cards can copy upgrade configuration.
- GUI shows installed upgrades and their effects.
- Debug tool reports effective multipliers.
- Recalculate stats when upgrades are added or removed.

## Edge Cases
- Adding upgrades while a machine is running.
- Exceeding stack limits.
- Conflicting upgrades (if any).
- Factory-wide vs per-slot application.

## Implementation Notes
- Keep upgrade logic in a shared component.
- Use data-driven multipliers.
- Support both machine and factory application.
