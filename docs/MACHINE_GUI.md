# Machine GUI

## Goals
- Consistent layout for all processing machines.
- Clear input, output, progress, energy, and chemical/fluid feedback.
- Support for upgrades, redstone, and side configuration.

## Layout
- Input slots on the left.
- Output slots on the right.
- Progress bar in the center.
- Energy gauge on the left or top.
- Chemical/fluid tanks as vertical gauges.
- Upgrade slots in a dedicated area.
- Redstone and security indicators.
- Side configuration button or widget.

## Widgets
- Progress bar with direction and tooltip.
- Energy and tank gauges with SI/mB formatting.
- Upgrade display.
- Status message area for insufficient resources or redstone.

## Interaction
- Right-click opens GUI.
- Configurator can open side config or cycle modes.
- JEI/EMI ghost ingredients supported.
- Shift-click for quick transfer.

## Integration
- Shared widgets for energy, tanks, and side config.
- Rate-limited sync for progress and levels.
- Config cards can copy relevant settings.

## Edge Cases
- Multiple chemical inputs.
- Factory-style machines (link to Factory GUI).
- Unpowered or redstone-disabled state.
