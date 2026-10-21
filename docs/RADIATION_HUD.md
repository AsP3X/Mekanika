# Radiation HUD for Mekanika

This document defines the player-facing radiation display. All content is original and conceptual.

## Goals

- Provide clear, escalating feedback on radiation exposure.
- Integrate with the radiation system.
- Remain non-intrusive at low levels and urgent at high levels.
- Support protection from gear modules.

## Display Elements

- Exposure bar or numerical value (accumulated radiation).
- Severity indicator: Safe / Low / Moderate / High / Critical.
- Color coding: green → yellow → orange → red.
- Optional Geiger-style ticks or sound intensity.
- Protection status if a radiation module is active.
- Decay indicator when exposure is falling.

## Layout

- Compact HUD element (corner or near hotbar).
- Expands or becomes more visible at higher severity.
- Tooltip or chat message on significant changes.
- Optional full screen overlay at critical levels.

## Behavior

- Updates from server-synced exposure value.
- Rate-limited to avoid spam.
- Escalating warnings as thresholds are crossed.
- Clear message when exposure begins to decay.

## Integration

- Reads from the radiation system.
- Gear modules reduce effective exposure and are shown.
- Debug tool can report local radiation sources.
- Config can adjust warning thresholds and HUD visibility.

## Edge Cases

- Multiple overlapping sources.
- Protection module removed mid-exposure.
- Creative mode immunity option.
- Very high exposure values.

## Implementation Notes

- Keep the HUD lightweight.
- Use clear color and sound cues.
- Document threshold values.
- Test with and without protection.

The radiation HUD makes nuclear content readable and manageable.
