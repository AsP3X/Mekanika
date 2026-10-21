# GUI Patterns — Clean-Room Design

This document defines consistent GUI patterns for Mekanika machines, factories, and multiblock structures. All content is original.

## Goals

- Provide a consistent, readable interface across all machines.
- Reuse shared widgets for energy, chemicals, fluids, progress, and side configuration.
- Support JEI/EMI integration where practical.
- Keep visual language aligned with the industrial style and tier colors.
- Minimize custom rendering cost.

## Shared Layout Principles

- Input slots on the left, output slots on the right, processing area in the center.
- Energy gauge on the left or bottom-left.
- Chemical and fluid gauges near relevant slots.
- Upgrade slots in a dedicated row or panel.
- Tabs or buttons for redstone control, security, and side configuration.
- Status messages or icons for failure states (insufficient energy, redstone disabled, full output).

## Shared Widgets

### Energy Gauge
- Vertical or horizontal bar showing stored energy vs capacity.
- Tooltip with exact values and rates.
- Color consistent with energy system.

### Chemical / Fluid Gauge
- Tank-style display with fill level and type name.
- Support for multiple tanks.
- Tooltip with amount, capacity, and rate.

### Progress Bar
- Horizontal bar showing operation progress.
- Clear direction (left to right).
- Optional overlay for speed upgrades.

### Side Configuration Widget
- Six-face representation or button that opens a side config screen.
- Color language matches side modes (input, output, energy, none).
- Accessible from the main GUI.

### Upgrade Display
- Shows installed upgrades and their counts.
- Tooltip explains effects.

## Machine GUIs

- Standard single-slot or multi-slot processing machines follow the shared layout.
- Input item slot(s), output item slot(s), and relevant chemical/fluid gauges.
- Progress bar between input and output.
- Energy gauge always visible.
- Config button for side configuration and redstone mode.

## Factory GUIs

- Multiple processing slots arranged in a grid.
- Shared energy and chemical gauges.
- Per-slot progress bars or a shared visualization.
- Auto-sort toggle.
- Slot count scales with factory tier (3/5/7/9).
- Upgrades affect the whole factory.

## Multiblock GUIs

- Opened from the controller or any structural block that proxies to the controller.
- Display aggregated stats (capacity, rate, temperature, burn rate, etc.).
- Port configuration accessible.
- Status indicators for formed state and failure reasons.
- Large structures may use tabs for different resource types.

## Consistency Rules

- Use the same slot size and spacing across machines.
- Tier color language appears on borders or accents where relevant.
- All gauges use the same tooltip format.
- Redstone and security indicators are always in the same location.
- Support ghost ingredient dragging from JEI/EMI where recipes are involved.
- Rate-limit client updates for dynamic elements.

## Implementation Recommendations

1. Create a shared base GUI class early.
2. Implement reusable gauge and progress widgets first.
3. Add the side configuration widget as a reusable component.
4. Keep machine GUIs thin; push common logic into the base.
5. Document expected GUI sizes and slot positions for texture artists.
6. Test with JEI/EMI integration early.
7. Provide clear failure feedback in the GUI.

## Edge Cases

- Factories with partial slot utilization.
- Multiblock GUIs when the structure is invalid.
- Rapid resource changes requiring rate-limited sync.
- Config card paste updating GUI state.
- Creative mode or security bypass affecting displayed options.

Consistent GUIs improve usability and reduce development time across the mod.
