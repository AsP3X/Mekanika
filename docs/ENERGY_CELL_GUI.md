# Energy Cell GUI

This document defines the GUI for Energy Cells, primarily used in the Induction Matrix and as portable or component energy storage.

## Goals

- Clear display of the cell’s individual capacity and current energy.
- Show contribution to the overall structure when part of an Induction Matrix.
- Consistent with Energy Core and other energy GUIs.
- Minimal but informative interface.

## Layout

### Individual Energy Cell
- Compact energy gauge showing the cell’s own capacity and fill level.
- Numerical display of stored energy and max capacity.
- Tier indicator (Basic / Advanced / Elite / Ultimate).
- Optional contribution preview if the cell is installed in a formed Matrix.

### When Installed in Induction Matrix
- The cell itself may not have a full GUI, or it opens a simplified view.
- Primary interaction is through the Matrix port or controller GUI.
- Cell GUI (if present) shows local stats and links to the structure.

## Widgets

- Small energy gauge with fill level and SI prefix formatting.
- Tier label or icon.
- Tooltip with exact values.
- Optional “Matrix Status” indicator if the cell is part of a formed structure.

## Interaction

- Right-click on a standalone cell opens its GUI.
- Right-click on a cell inside a formed Matrix may forward to the controller GUI or show a restricted view.
- Configurator can be used for any applicable configuration.
- No complex side configuration on individual cells (handled by Matrix ports).

## Integration

- Uses the shared energy container system.
- Aggregated into Induction Matrix stats at formation time.
- Syncs its own energy level when standalone.
- Debug tool can report individual cell contribution.

## Edge Cases

- Cell removed from a formed Matrix.
- Mixed-tier cells in the same structure.
- Empty or full cell states.
- Cell placed outside a valid structure.

## Implementation Notes

- Reuse the shared energy gauge widget.
- Keep the GUI simple since most interaction happens at the Matrix level.
- Rate-limit sync for standalone cells.
- Document expected size for texture planning.

This GUI provides clear feedback for individual energy cells while keeping the primary user experience focused on the Induction Matrix controller and ports.
