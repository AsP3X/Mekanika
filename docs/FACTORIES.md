# Factory System

## Goals
- Parallel processing of the same recipe across multiple independent slots.
- Shared energy, chemical, and fluid inputs for efficiency.
- Auto-sort for player convenience.
- Clear GUI feedback for multi-slot progress.

## Slot Model
- Slot count scales with tier: 3 (Basic), 5 (Advanced), 7 (Elite), 9 (Ultimate).
- Each slot has its own progress and can process independently.
- Shared recipe selection — all slots process the same recipe.
- Energy and chemical consumption is summed across active slots.

## Auto-Sort
- Toggleable feature that moves matching inputs into empty slots.
- Runs periodically or on inventory change.
- Respects filters and side configuration.

## Upgrades
- Speed, energy, and gas upgrades apply to the whole factory.
- Effects are calculated once and applied to all slots.
- Upgrade slots are shared.

## GUI
- Grid or row layout showing all process slots.
- Shared energy and chemical gauges.
- Auto-sort toggle.
- Progress visualization per slot or aggregated.

## Integration
- Reuses base machine recipe handlers and side configuration.
- Factories can be used in place of single machines in processing chains.
- Debug tool reports active slot count and efficiency.

## Edge Cases
- Partial inputs across slots.
- Recipe changes while slots are processing.
- Full output inventory blocking progress.
- Mixed upgrade effects.

## Implementation Notes
- Track progress per slot independently.
- Batch energy and chemical consumption.
- Rate-limit auto-sort checks.
- Support config cards for factory settings.
