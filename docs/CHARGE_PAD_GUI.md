# Charge Pad GUI for Mekanika

Pixel positions, screen sizes, and required widgets are in [GUI_LAYOUTS.md](GUI_LAYOUTS.md).

This document defines the Charge Pad and related portable charging interfaces. All content is original and conceptual.

## Goals

- Provide a simple way to charge energy tablets, gear, and gadgets.
- Display charge progress clearly.
- Integrate with the shared energy system.
- Support side configuration for energy input.

## Layout

- Central charge slot for the item being charged.
- Energy bar showing the pad’s buffer or transfer rate.
- Progress indicator for the item being charged.
- Rate display (energy per tick).
- Side configuration button.
- Redstone control.
- Player inventory at the bottom.

## Behavior

- Accepts energy from cables or other sources on configured faces.
- Transfers energy into the item in the charge slot.
- Stops when the item is full or energy is unavailable.
- Supports energy tablets, gear pieces, and compatible gadgets.
- The screen has one charge slot.

## Integration

- Uses the shared energy container.
- Side configuration controls input faces.
- Config cards can copy settings.
- Debug tool reports buffer and transfer rate.
- Syncs charge progress efficiently.

## Edge Cases

- Item removed mid-charge.
- Insufficient energy input.
- Incompatible item in slot.
- Creative infinite energy.
- Rapid item swapping.

## Implementation Notes

- Reuse energy bar and slot widgets.
- Validate item energy capability before charging.
- Provide clear full/empty/charging states.
- Document charge rate values in config.

The Charge Pad provides convenient early- and mid-game charging for portable items.
