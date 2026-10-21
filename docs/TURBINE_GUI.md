# Industrial Turbine GUI for Mekanika

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

This document defines the GUI for the Industrial Turbine. All content is original and conceptual.

## Goals

- Display steam input, energy output, and efficiency clearly.
- Show blade, coil/disperser, and vent status.
- Provide formation and component summary.
- Integrate with the Multiblock Data view.
- Highlight limiting factors.

## Layout

- Large energy output rate display.
- Steam consumption rate and flow gauge.
- Efficiency percentage or limiting-factor indicator.
- Component summary: rotor blades, coils or dispersers, vents.
- Formed status, dimensions, and volume.
- Port configuration access.
- Redstone and security controls.
- Multiblock Data tab or section.

## Widgets

- Energy production gauge.
- Steam flow gauge.
- Component counters by type.
- Status banner (forming / active / steam-limited / blade-limited / offline).
- Tooltips with exact values.

## Interactions

- Opens from any structural block or the controller.
- Configurator adjusts energy and steam ports.
- Most stats are read-only; configuration happens via ports and side config.

## Integration

- Reads aggregated data from Turbine MultiblockData.
- Links conceptually to boiler steam production.
- Debug tool reports the same core stats.
- Syncs rates efficiently (rate-limited).

## Edge Cases

- Insufficient steam.
- Missing or insufficient blades.
- Vent-limited maximum flow.
- Unformed structure.
- Rapid steam changes.

## Implementation Notes

- Reuse energy and status widgets.
- Clearly indicate the current limiting factor.
- Document expected display ranges.
- Test with various blade and vent counts.

This GUI makes turbine performance understandable and helps players optimize their builds.
