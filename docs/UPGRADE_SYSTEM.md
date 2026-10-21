# Upgrade System

## Goals
- Shared upgrade component for machines and factories.
- Data-driven effects on speed, energy, gas, etc.

## Upgrades
- Speed, Energy, Gas, Filter, Anchor, Muffling.
- Stack limits per machine.
- Effects calculated from base values + upgrade count.
- Factories apply upgrades to all slots or shared systems.

## Implementation
- Store upgrades in a component or attachment.
- Recalculate derived stats when upgrades change.
- Config cards can copy upgrade configuration.
