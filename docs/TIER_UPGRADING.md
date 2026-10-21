# Tier Upgrading for Mekanika

This document defines how machines and components are upgraded between tiers. All content is original and conceptual.

## Goals

- Provide a clear Basic → Advanced → Elite → Ultimate path.
- Preserve player configuration and contents where possible.
- Keep upgrade recipes data-driven.
- Integrate with factories and multiblocks where applicable.

## Tier Installers

- Craftable items that convert a machine to the next tier.
- Applied via right-click or a dedicated GUI slot.
- Consume the installer item on success.
- Visual change via tier-specific textures.

## Preservation Rules

- Side configuration is preserved.
- Redstone mode and security settings are preserved.
- Installed upgrades are preserved if compatible.
- Items in input/output slots are preserved or dropped safely.
- Energy, fluid, and chemical contents are preserved.
- Factory slot count increases with tier.

## Multiblock and Component Upgrading

- Individual components (cells, providers, blades) can be upgraded or replaced.
- Structure must be reformed after major internal changes.
- Aggregated stats recalculate on reformation.

## Integration

- Recipes defined in data files.
- JEI shows upgrade paths.
- Config can adjust installer costs if desired.
- Debug tool can report current tier.

## Implementation Notes

1. Implement after base machines exist.
2. Test content and config preservation carefully.
3. Handle partial failures (e.g., inventory full) cleanly.
4. Document which blocks support upgrading.
5. Provide clear success and failure feedback.

## Edge Cases

- Upgrading while processing.
- Incompatible upgrades after tier change.
- Creative mode infinite installers.
- Upgrading a machine that is part of a network.

Tier upgrading supports smooth progression without forcing players to rebuild setups.
