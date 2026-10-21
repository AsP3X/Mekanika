# Factories for Mekanika

This document provides detailed, clean-room design guidance for factory machines. All content is original and conceptual.

## Goals

- Parallel processing with shared recipe logic and efficient resource use.
- Auto-sort to reduce manual inventory management.
- Clear interaction with upgrades, energy, and chemicals.
- Consistent behavior with base machines.
- Data-driven slot counts and rates.

## Core Concepts

### Process Slots
- Tiered slot count (e.g., Basic 3, Advanced 5, Elite 7, Ultimate 9).
- Each slot tracks its own input, output, progress, and current recipe.
- Shared recipe lookup across slots for performance.
- Slots can process independently or share resources.

### Resource Sharing
- Energy consumption is summed across all active slots each tick.
- Chemical and fluid consumption is shared or per-slot (document the chosen model; shared is simpler).
- Auto-sort moves matching items from a shared input into empty slots when inventory changes.
- Output slots can be shared or individual.

### Upgrades
- Applied to the factory as a whole.
- Speed upgrades increase processing rate and resource consumption.
- Energy upgrades reduce energy use.
- Chemical upgrades improve efficiency or throughput.
- Effects scale with the number of active slots.

## GUI

- Multiple process slots with individual progress bars.
- Shared energy and chemical gauges.
- Auto-sort toggle.
- Upgrade slots.
- Side configuration and control buttons.
- Clear indication of which slots are active or blocked.

## Integration Points

- Side configuration and control systems (redstone, security).
- Networks for item and chemical movement.
- Recipe system for shared lookup.
- JEI/EMI for factory recipes.
- Config cards for copying settings.
- Debug tool for slot status and consumption.

## Implementation Recommendations

1. Implement after the base machine, recipe system, and upgrades are stable.
2. Add auto-sort early for quality of life.
3. Decide and document shared vs. per-slot resource model.
4. Integrate upgrades so they scale correctly with slot count.
5. Make slot counts and multipliers data-driven.
6. Provide clear GUI feedback for blocked slots.

## Edge Cases

- Mixed inputs across slots.
- Auto-sort conflicts or loops.
- Upgrade changes while running.
- Full output slots blocking progress.
- Chemical or energy shortage affecting only some slots.
- Config card paste on a factory with different slot count.

## Player Feedback

- Progress bars per slot.
- Status messages for why a slot is not processing.
- Clear auto-sort behavior.
- Tooltip for upgrade effects.

This design provides efficient parallel processing while remaining consistent with base machine behavior and easy to balance.
