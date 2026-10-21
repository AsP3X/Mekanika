# Tank GUI (Chemical and Fluid)

## Goals
- Clear display of stored chemical or fluid type and amount.
- Support for filtering, dump modes, and side configuration.

## Layout
- Large vertical gauge showing fill level and type.
- Numerical amount and capacity.
- Filter indicator or selector.
- Dump mode button (None / Excess / All).
- Side configuration widget.
- Input/output item slots for buckets or portable tanks if applicable.

## Widgets
- Colored gauge matching the fluid/chemical.
- Tooltip with exact values.
- Dump mode toggle with clear feedback.

## Interaction
- Right-click opens GUI.
- Configurator for side modes.
- Bucket interaction if enabled.

## Integration
- Uses shared tank and side config systems.
- Works for both machine tanks and standalone tanks.
- Dynamic Tank uses Multiblock Data GUI with tank-specific extensions.
