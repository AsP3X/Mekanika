# Side Configuration and Configurator — Clean-Room

## Data Model
- Per-transmission-type configuration (Items, Fluids, Chemicals, Energy, Heat).
- For each type: map of RelativeSide → DataType (None, Input, Output, Extra, etc.).
- Ejecting flag per type.
- Supported types list.
- SideData descriptors for valid modes and colors.

## Configurator Tool
- Modes: Configurate (cycle side mode), Wrench, etc.
- Shift + right-click cycles modes for the hovered transmission type.
- Right-click reports current mode.
- Works on machines and transmitters.

## Implementation Notes
- Store config in a TileComponentConfig equivalent.
- Notify neighbors and update capabilities when a side changes.
- Support copy/paste via config cards.
- GUI shows colored sides matching the color scheme (grey=none, dark red=input, dark blue=output, green=energy, etc.).

## Lessons
- Side config is foundational UX. Implement early.
- Keep the model data-driven so new transmission types can be added.
- Configurator should be a multi-mode tool from the start.
