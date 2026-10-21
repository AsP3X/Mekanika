# Side Configuration and Configurator — Expanded Clean-Room

This document defines the conceptual design for side configuration in Mekanika. All content is original.

## Data Model

- One configuration entry per TransmissionType (Items, Fluids, Chemicals, Energy, Heat).
- For each type:
  - Map of RelativeSide → DataType (None, Input, Output, Energy, Extra, etc.).
  - Ejecting flag (boolean).
  - CanEject flag (whether ejection is allowed).
- Supported TransmissionTypes list (declared by the machine).
- SideData descriptors: valid modes, display names, and colors for the GUI.
- Entire configuration is serializable (NBT or data component) for config cards and persistence.

## Configurator Tool

- Multi-mode item:
  - Configurate: Cycle side mode for the active transmission type on the hovered face.
  - Wrench: Rotate machine.
  - Optional: Other modes (e.g., network reader).
- Interaction:
  - Right-click: Report current mode.
  - Shift + right-click: Cycle to next mode.
  - Works on machines and transmitters.
- Active transmission type can be selected via the tool or GUI.

## GUI Behavior

- Tabs or buttons for each supported TransmissionType.
- Visual representation of the six faces with color-coded modes.
- Example color language:
  - None: grey / black
  - Input: dark red
  - Output: dark blue
  - Energy: green
  - Extra / Chemical: purple or cyan
- Clicking a face cycles its mode for the current type.
- Auto-eject toggle per type.

## Implementation Notes

- Store configuration in a reusable component (TileComponentConfig equivalent) attached to the base machine tile.
- On side change:
  - Update capabilities / handlers for the affected side.
  - Notify neighbors of capability changes.
  - Mark the tile dirty.
- Machines declare supported types at construction; the UI and Configurator only show relevant options.
- Support default configurations that can be overridden.
- Config cards copy the full side configuration (and optionally upgrades/security).

## Integration Points

- **Machines**: All processing machines use side config for items, energy, and chemicals.
- **Transmitters**: Tubes and cables use side config for pull/push and filtering.
- **Multiblocks**: Ports and valves expose side config where relevant.
- **Factories**: Side config applies to the shared inputs/outputs.
- **Security**: Side config changes may require public access or ownership.

## Implementation Recommendations

1. Implement the side configuration component early, before most machines.
2. Make the data model data-driven so new transmission types can be added without core changes.
3. Provide a shared GUI widget for side config to avoid duplication.
4. Ensure capability updates happen immediately on side change.
5. Add config card support once the basic system works.
6. Include side config status in the debug tool.

## Edge Cases

- Changing side config while resources are flowing.
- Unsupported transmission types on a machine.
- Config card paste onto a machine with different supported types.
- Multiple players configuring the same machine.
- Persistence of side config across upgrades or tier changes.

Side configuration is foundational UX. Implementing it early and keeping it consistent will improve the feel of the entire mod.
