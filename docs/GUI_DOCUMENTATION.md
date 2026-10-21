# GUI Documentation for Mekanika

This document defines consistent GUI patterns for machines, factories, multiblocks, and supporting interfaces. All designs are original and conceptual.

## Goals

- Consistent, readable interfaces across all machines and structures.
- Clear feedback for energy, chemicals, fluids, progress, and status.
- Support for side configuration, upgrades, and redstone/security.
- JEI/EMI compatibility (ghost ingredients, recipe lookup).
- Accessible from any valid block on multiblocks.

## Shared Layout Conventions

### Machine GUIs
- **Input slots**: Left side.
- **Output slots**: Right side.
- **Processing area**: Center (progress bar or animation).
- **Upgrade slots**: Bottom or dedicated panel.
- **Energy bar**: Left or top, with numerical tooltip.
- **Chemical/Fluid tanks**: Vertical gauges with type and amount.
- **Redstone and Security**: Small indicators or buttons.

### Factory GUIs
- Multiple process slots arranged in a grid or rows.
- Shared energy and chemical displays.
- Auto-sort toggle clearly visible.
- Slot count visually scales with tier (3/5/7/9).

### Multiblock GUIs
- Opened from any structural block (forwards to controller).
- Prominent display of aggregated stats (capacity, rate, efficiency).
- Port or valve configuration accessible.
- Formation status and component summary.

## Widgets and Elements

### Progress Bars
- Clear direction (left-to-right or bottom-to-top).
- Color-coded by type (energy green, chemical purple, etc.).
- Tooltip with exact values.

### Tank Gauges
- Vertical fill with fluid/chemical color.
- Numerical amount and capacity on hover.
- Filter indicator if applicable.

### Side Configuration Widget
- Visual representation of the six faces.
- Color language matching the Configurator (grey = none, dark red = input, dark blue = output, green = energy, etc.).
- Clickable to cycle modes or open detailed config.

### Buttons and Toggles
- Redstone mode cycle button.
- Security mode indicator.
- Auto-eject or auto-sort toggle.
- Config card copy/paste buttons where relevant.

## Interaction Patterns

- Right-click opens GUI.
- Configurator can open side config directly or cycle modes.
- JEI/EMI ghost ingredients can be dragged into slots.
- Shift-click for quick transfer.
- Tooltips provide exact numbers and status.

## Multiblock Specifics

- Any casing or port can open the controller GUI.
- Display current formation status and last failure reason if unformed.
- Show component counts (cells, providers, blades, etc.).
- Allow port configuration without breaking formation.

## Consistency Rules

- Use the same color language across all GUIs and the Configurator.
- Energy is always shown in Joules with SI prefixes.
- Chemicals and fluids show type name and amount.
- Progress bars use consistent timing and direction.
- All GUIs support the same redstone and security indicators.

## Implementation Notes

- Prefer reusable widget classes for tanks, progress bars, and side config.
- Sync only the data needed for display (rate-limited).
- Support both mouse and keyboard navigation where practical.
- Document expected GUI sizes so textures and layouts remain consistent.

## Edge Cases

- Unformed multiblock GUI (show error state).
- Security-restricted access (clear denial message).
- Rapid changes (energy/chemical levels) without flickering.
- Factory with mixed slot states.
- Configurator overlay when GUI is open.

## Player Feedback

- Clear status messages for insufficient resources or redstone disabled.
- Visual indicators for active processing.
- Hover tooltips with full details.
- Optional sound feedback for mode changes.

This document provides the foundation for consistent, usable GUIs across all Mekanika machines and structures.
