# Factory System

## Goals
- Parallel processing of the same recipe across multiple slots.
- Shared energy and chemical inputs where practical.
- Auto-sort for convenience.

## Design
- Slot count scales with tier (3/5/7/9).
- Each slot has independent progress but shared recipe logic.
- Energy and chemical consumption summed across active slots.
- Auto-sort moves matching inputs into empty slots.
- Upgrades apply to the whole factory or per-slot based on balance.

## GUI
- Clear visualization of multiple slots.
- Shared tank and energy displays.
- Auto-sort toggle.

## Integration
- Reuses base machine recipe handlers.
- Side configuration for shared inputs/outputs.
- Debug tool shows active slot count and efficiency.
