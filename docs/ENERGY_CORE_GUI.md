# Energy Core / Energy Cube GUI Design for Mekanika

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

This document provides detailed, clean-room design guidance for the GUI of the Energy Core (Energy Cube) and related energy storage interfaces. All content is original and conceptual.

## Goals

- Clear display of current energy, capacity, and transfer rates.
- Easy configuration of input and output faces.
- Integration with side configuration and the Configurator tool.
- Visual feedback for charging, discharging, and network activity.
- Support for tiered Energy Cubes and large storage (Induction Matrix ports).
- Consistent with overall GUI patterns.

## Layout

### Standard Energy Cube GUI
- Large central energy gauge showing current amount and capacity.
- Input rate and output rate displays.
- Side configuration button or integrated face overview.
- Energy cubes have no upgrade slots and no item slots.
- Redstone control and security indicators.
- Player inventory at the bottom.

### Large Storage (Induction Matrix Port)
- Aggregated capacity and rate from the structure.
- Controller status and formation indicator.
- Port-specific I/O configuration.
- Link to full structure statistics.

## Widgets

- Energy gauge with fill level, numeric value, and SI prefix formatting (kJ, MJ, etc.).
- Input/output rate indicators.
- Side configuration widget showing current energy faces (green for energy input/output).
- Status light for activity (charging, discharging, idle).
- Tooltip on hover for exact values and rates.

## Interactions

- Right-click opens the GUI.
- Configurator can cycle energy modes on faces or open the side config screen.
- Shift-click or dedicated button for side configuration.
- Comparator output based on fill level.
- Config card can copy energy-related side config and redstone mode.

## Visual Feedback

- Gauge fill animates or updates smoothly.
- Color coding consistent with side configuration (green for energy).
- Particles or texture animation on the block when actively transferring (client-side).
- Clear message if the cube cannot accept or output energy due to configuration or redstone.

## Integration Points

- Side configuration drives which faces accept or emit energy.
- Networks report throughput and buffer status in the debug tool; GUI shows local rates.
- Upgrades (if supported) affect capacity or rate and are displayed.
- Security and redstone control apply as in other machines.
- Client sync rate-limited for the gauge.

## Implementation Recommendations

1. Use the shared energy gauge component.
2. Integrate the side configuration widget early.
3. Support both small Energy Cubes and large storage ports with the same base layout where possible.
4. Rate-limit sync for the energy value.
5. Provide clear tooltips and status messages.
6. Follow the color language from the GUI patterns document.

## Edge Cases

- Creative or infinite energy sources.
- Rapid charging and discharging.
- Side configuration changes while the GUI is open.
- Chunk unload with energy in transit.
- Mixed input and output on the same network.

## Player Feedback

- Tooltip shows exact energy amount, capacity, and rates.
- Status indicator for current mode (charging, idle, full, empty).
- Clear indication when redstone or configuration is blocking transfer.
- Debug tool complements the GUI with network-level information.

This design provides a clear, consistent, and informative interface for all energy storage in Mekanika.
