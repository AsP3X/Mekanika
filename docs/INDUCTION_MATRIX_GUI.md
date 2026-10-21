# Induction Matrix GUI

This document defines the GUI for the Induction Matrix, the large-scale energy storage multiblock.

## Goals

- Clear display of total capacity, current energy, and transfer rate.
- Overview of installed cells and providers.
- Port configuration access.
- Formation status and component summary.
- Consistent with other multiblock and energy GUIs.

## Layout

### Main View
- **Large Energy Gauge**: Shows total stored energy vs. aggregated capacity.
- **Transfer Rate**: Current input/output throughput.
- **Component Summary**: Counts of cells and providers by tier.
- **Efficiency or Status**: Optional indicator for structure health.
- **Port List or Configuration**: Access to configure individual ports.
- **Redstone and Security**: Indicators if applicable.

### Tabs or Sections (optional)
- Overview (gauge + summary)
- Components (detailed cell/provider list)
- Ports (side configuration for each port)
- Statistics (historical or rate data)

## Widgets

- Large energy gauge with SI prefix formatting.
- Component count display (e.g., “12 Elite Cells, 4 Advanced Providers”).
- Port configuration widget or list.
- Status light for formed / unformed / error.
- Tooltips with exact values.

## Interaction

- Right-click on any structural block or port opens the controller GUI.
- Configurator can cycle port modes or open side config.
- Buttons or tabs for switching views.
- Config card support for port configurations.

## Integration

- Aggregated stats calculated at formation and cached.
- Ports use the shared side configuration system.
- Energy value owned by the controller.
- Debug tool reports the same aggregated values.
- Syncs efficiently (rate-limited for the gauge).

## Edge Cases

- Unformed structure (show error state and last failure).
- Mixed-tier components.
- Multiple ports with different configurations.
- Rapid energy changes.
- Security restrictions.

## Implementation Notes

- Reuse energy gauge, side config, and status widgets.
- Controller owns the GUI and data.
- Rate-limit client sync for energy level.
- Support both simple and tabbed layouts depending on complexity.

## Player Feedback

- Clear capacity and rate display.
- Component summary so players understand their build.
- Formation error messages if unformed.
- Status indicators for active transfer.

This GUI provides a comprehensive interface for the Induction Matrix, making large-scale energy storage easy to monitor and configure.
